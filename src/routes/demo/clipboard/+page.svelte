<script lang="ts">
	import { onMount } from 'svelte';

	const images = import.meta.glob('./clipboard.assets/*.(svg|png)', {
		as: 'url',
		eager: true
	});

	type ClipboardContents = {
		type: string;
		value: string;
		displayType: 'text' | 'html' | 'image';
	}[];

	let wasCopied = false;
	let wasPasted = false;
	let clipboardContents: ClipboardContents[] = [];

	let supportsSvg: undefined | boolean = undefined;
	let supportsWebFormats: undefined | boolean = undefined;
	let supportsMultipleClipboardItems: undefined | boolean = undefined;
	let supportsPresentationStyle: undefined | boolean = undefined;
	let supportsClipboardItem: undefined | boolean = undefined;
	let supportsClipboardItemConstructor: undefined | boolean = undefined;
	let supportsSupportsStatic: undefined | boolean = undefined;
	let supportsTypes: undefined | boolean = undefined;

	const onPaste = async () => {
		const contents: ClipboardContents[] = [];
		const items = await navigator.clipboard.read();
		for (const item of items) {
			contents.push([]);
			const contentsRow = contents[contents.length - 1];

			supportsTypes = 'types' in item;
			for (const itemType of item.types) {
				if (
					wasCopied &&
					(!('presentationStyle' in item) || item.presentationStyle !== 'attachment')
				) {
					supportsPresentationStyle = false;
				}

				const blob = await item.getType(itemType);
				// empty clipboards
				if (!blob) {
					continue;
				}

				if (/(web )?image\//.test(itemType)) {
					if (itemType === 'image/svg+xml') {
						supportsSvg = true;
					}
					contentsRow.push({
						type: itemType,
						value: URL.createObjectURL(blob),
						displayType: 'image'
					});
				} else if (/(web )?text\//.test(itemType)) {
					if (itemType === 'web text/custom-stars') {
						supportsWebFormats = true;
					}
					contentsRow.push({
						type: itemType,
						value: await blob.text(),
						displayType: 'text'
					});
				} else if (/(web )?html\//.test(itemType)) {
					contentsRow.push({
						type: itemType,
						value: await blob.text(),
						displayType: 'html'
					});
				}
			}
		}

		if (wasCopied) {
			supportsMultipleClipboardItems = items.length > 1;
			// Safari does not throw
			if (supportsWebFormats === undefined) {
				supportsWebFormats = false;
			}
			// Safari does not throw
			if (supportsSvg === undefined) {
				supportsSvg = false;
			}

			wasCopied = false;
		}
		wasPasted = true;
		clipboardContents = contents.filter((content) => content.length > 0);
	};

	async function tryPasting(items: Record<string, string | Blob>[]) {
		try {
			let clipboardItems;
			try {
				clipboardItems = items.map(
					(item) => new ClipboardItem(item, { presentationStyle: 'attachment' })
				);
				supportsClipboardItemConstructor = true;
			} catch (e: unknown) {
				supportsClipboardItemConstructor = false;
				throw e;
			}

			await navigator.clipboard.write(
				items.map((item) => new ClipboardItem(item, { presentationStyle: 'attachment' }))
			);
		} catch (e: unknown) {
			if (!(e instanceof Error)) {
				throw e;
			}

			if (
				e.message.includes('Support for multiple ClipboardItems is not implemented.') ||
				e.message.includes('Clipboard write is only supported with one ClipboardItem at the moment')
			) {
				supportsMultipleClipboardItems = false;
				items.pop();
				return await tryPasting(items);
			}

			if (e.message.includes('image/svg+xml')) {
				supportsSvg = false;
				delete items[0]['image/svg+xml'];
				return await tryPasting(items);
			}

			if (e.message.includes('web text/custom-stars')) {
				supportsWebFormats = false;
				delete items[0]['web text/custom-stars'];
				return await tryPasting(items);
			}
			throw e;
		}
	}

	async function onCopy(stars: string) {
		wasCopied = true;
		wasPasted = false;

		const blob = await fetch(images[`./clipboard.assets/${stars}.png`]).then((r) => r.blob());
		const blobSvg = await fetch(images[`./clipboard.assets/${stars}.svg`]).then((r) => r.blob());
		// safari workaround https://stackoverflow.com/a/77517883
		setTimeout(async () => {
			const items = [
				{
					'text/plain': new Blob([`${stars}.png`], { type: 'text/plain' }),
					[blob.type]: blob,
					[blobSvg.type]: blobSvg,
					'web text/custom-stars': new Blob([stars], { type: 'web text/custom-stars' })
				},
				{
					'text/plain': new Blob([`${stars}.png`], { type: 'text/plain' }),
					[blob.type]: blob,
					[blobSvg.type]: blobSvg,
					'web text/custom-stars': new Blob([stars], { type: 'web text/custom-stars' })
				}
			];
			await tryPasting(items);
		}, 0);
	}

	onMount(() => {
		supportsClipboardItem = 'ClipboardItem' in window;
		if (supportsClipboardItem) {
			supportsSupportsStatic = 'supports' in ClipboardItem;
		} else {
			supportsSupportsStatic = false;
		}

		if (supportsSupportsStatic) {
			//@ts-expect-error
			supportsSvg = ClipboardItem.supports('image/svg+xml');
			//@ts-expect-error
			supportsWebFormats = ClipboardItem.supports('web text/custom-stars');
		}
	});

	function status(status: undefined | boolean) {
		if (status === undefined) {
			return '❔';
		}
		return status ? '✅ ' : '❌';
	}

	function onWindowCopy() {
		let text = window.getSelection()?.toString() ?? '';

		if (!text.length) {
			onCopy('five-stars');
		}
	}
</script>

<svelte:window on:paste={onPaste} on:copy={onWindowCopy} />

<h1>Clipboard API</h1>
<p>
	The Clipboard API is a replacement for the deprecated document.execCommand() and is currently
	under development. It allows for advanced features like customized content and multi-format data support, enabling richer interactions.
</p>
<p>
	
	The API enhances control and security by ensuring precise data transfer and protecting against unauthorized clipboard interference.
</p>
<h2>Inconsistent Support</h2>
<p>
	In Safari, unsupported formats don't result in an error, and you can still proceed with copying
	text. However, Chrome and Firefox might throw an error when faced with unsupported formats.
</p>
	<p>
	Additionally, Safari and Firefox may prompt you before pasting text, whereas Chrome might request
	permissions only once.
</p>
<h2>Demo </h2>
<p>You can use this demo to check what is supported by your browser.</p>
<hr />

<section class="clipboard-actions">
	<button class="button" on:click={onPaste}>Paste</button>

	<button class="button" on:click={onCopy.bind(null, 'one-star')}> copy one star </button>
	<button class="button" on:click={onCopy.bind(null, 'two-stars')}> copy two stars </button>
	<button class="button" on:click={onCopy.bind(null, 'three-stars')}> copy three stars </button>
	<button class="button" on:click={onCopy.bind(null, 'four-stars')}> copy four stars </button>
	<button class="button" on:click={onCopy.bind(null, 'five-stars')}> copy five stars </button>
</section>
<section>
	<div class="table__container">
		<table>
			<caption>Clipboard API support for current browser</caption>
			<tbody>
				<tr>
					<td>Feature</td>
					<td>Supported</td>
					<td>Details</td>
				</tr>
				<tr>
					<td> ClipboardItem </td>
					<td>
						{status(supportsClipboardItem)}
					</td>
					<td>Supported by all modern browsers</td>
				</tr>
				<tr>
					<td> ClipboardItem (constructor) </td>
					<td>
						{status(supportsClipboardItemConstructor)}
					</td>
					<td>Copy stars to check if this feature is supported</td>
				</tr>
				<tr>
					<td>Static ClipboardItem.supports() </td>
					<td>
						{status(supportsSupportsStatic)}
					</td>
					<td>—</td>
				</tr>
				<tr>
					<td> ClipboardItem.types() </td>
					<td>
						{status(supportsTypes)}
					</td>
					<td>Copy and paste stars to check if this feature is supported</td>
				</tr>
				<tr>
					<td> Multiple items </td>
					<td>
						{status(supportsMultipleClipboardItems)}
					</td>
					<td>Copy stars to check if this feature is supported</td>
				</tr>
				<tr>
					<td>image/svg+xml</td>
					<td>
						{status(supportsSvg)}
					</td>
					<td>Copy and paste stars to check if this feature is supported</td>
				</tr>
				<tr>
					<td>Custom web formats</td>
					<td>
						{status(supportsWebFormats)}
					</td>
					<td>Copy and paste stars to check if this feature is supported</td>
				</tr>
				<tr>
					<td>presentationStyle</td>
					<td>
						{status(supportsPresentationStyle)}
					</td>
					<td>Copy and Paste stars to check if this feature is supported</td>
				</tr>
			</tbody>
		</table>
	</div>
	{#if wasPasted && clipboardContents.length === 0}
		<div class="callout info">
			<p class="callout__paragraph">Nothing to paste</p>
		</div>
	{:else if wasPasted}
		<div class="table__container">
			<table>
				<caption>Paste details</caption>
				<tbody>
					{#each clipboardContents as clipboardItems, index}
						<tr>
							<th colspan="2">
								ClipboardItem {index + 1}
							</th>
						</tr>
						{#each clipboardItems as clipboardItem}
							<tr>
								<td>
									{clipboardItem.type}
								</td>
								{#if clipboardItem.displayType === 'text'}
									<td>{clipboardItem.value}</td>
								{:else if clipboardItem.displayType === 'image'}
									<td><img height="60" src={clipboardItem.value} alt="" /></td>
								{:else if clipboardItem.displayType === 'html'}
									<td><code>{clipboardItem.value}</code></td>
								{/if}
							</tr>
						{/each}
					{/each}
				</tbody>
			</table>
		</div>
	{/if}
</section>

<style>
	.clipboard-actions {
		display: flex;
		gap: 8px;
		justify-content: flex-end;
		flex-wrap: wrap;
	}
	.clipboard-actions button:first-of-type {
		margin-right: auto;
	}
</style>
