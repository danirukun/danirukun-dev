<script lang="ts">
  import { LinkPreview } from 'bits-ui';

  let className: string | undefined = undefined;
  export let href: string;
  export let contentClass: string | undefined = undefined;
  export { className as class };
</script>

<LinkPreview.Root>
  <LinkPreview.Trigger {href} class={className}>
    <slot />
  </LinkPreview.Trigger>
  <LinkPreview.Content class={contentClass}
    >
    <slot name="preview" />
  </LinkPreview.Content>
</LinkPreview.Root>


