<script lang="ts">
	import { Popover } from "bits-ui";
	import Info from "phosphor-svelte/lib/Info";
	import type { Component } from "svelte";
	import Code from "$lib/components/markdown/code.svelte";
	import type { DataAttrSchema } from "$lib/types/index.js";
	import { parseMarkdown } from "$lib/utils/markdown.js";

	let { attr }: { attr: DataAttrSchema } = $props();
</script>

{#snippet StringType()}
	<Code class="bg-transparent px-0">{parseMarkdown(attr.value ?? "''")}</Code>
{/snippet}

{#snippet DefinitionContent({ definition }: { definition: Component })}
	{@const Definition = definition}
	<Popover.Content
		preventScroll={false}
		side="top"
		sideOffset={10}
		class="rounded-card bg-background shadow-popover z-50 max-h-[400px] overflow-auto"
	>
		<div class="**:data-line:pr-2.5! [&_pre]:my-0! [&_pre]:mb-0! [&_pre]:mt-0!">
			<Definition />
		</div>
	</Popover.Content>

	<span aria-hidden="true" class="hidden">
		- {attr.value ?? "''"}
	</span>
{/snippet}

<div class="flex items-center gap-1.5">
	{#if !attr.isEnum}
		{@render StringType()}
	{:else}
		<Code class="bg-transparent px-0">enum</Code>
		<Popover.Root>
			<Popover.Trigger
				data-llm-ignore
				class="rounded-button text-muted-foreground focus-visible:ring-foreground focus-visible:ring-offset-background focus-visible:outline-hidden inline-flex items-center justify-center transition-colors focus-visible:ring-2 focus-visible:ring-offset-2"
			>
				<Info class="size-4" weight="bold" />
				<span class="sr-only">See type definition</span>
			</Popover.Trigger>
			{#if attr.definition}
				{@render DefinitionContent({ definition: attr.definition })}
			{/if}
		</Popover.Root>
	{/if}
</div>
