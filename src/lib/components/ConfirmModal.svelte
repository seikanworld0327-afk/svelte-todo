<script lang="ts">
  type Props = {
    open: boolean;
    title: string;
    message: string;
    confirmLabel?: string;
    cancelLabel?: string;
    onConfirm: () => void;
    onCancel: () => void;
  };

  let {
    open,
    title,
    message,
    confirmLabel = '削除する',
    cancelLabel = 'キャンセル',
    onConfirm,
    onCancel
  }: Props = $props();

  function handleKeydown(e: KeyboardEvent) {
    if (e.key === 'Escape') {
      onCancel();
    }
  }
</script>

<svelte:window onkeydown={open ? handleKeydown : undefined} />

{#if open}
  <div
    class="fixed inset-0 z-50 flex items-center justify-center bg-black/50 p-4"
    role="dialog"
    aria-modal="true"
    aria-labelledby="confirm-modal-title"
  >
    <div class="bg-white rounded-lg shadow-xl p-6 w-full max-w-sm">
      <h2 id="confirm-modal-title" class="text-lg font-bold text-gray-800 mb-2">{title}</h2>
      <p class="text-sm text-gray-600 mb-6">{message}</p>
      <div class="flex justify-end gap-2">
        <button
          type="button"
          onclick={onCancel}
          class="px-4 py-2 text-gray-600 font-semibold rounded hover:bg-gray-100"
        >
          {cancelLabel}
        </button>
        <button
          type="button"
          onclick={onConfirm}
          class="px-4 py-2 bg-red-500 text-white font-semibold rounded shadow-md hover:bg-red-600"
        >
          {confirmLabel}
        </button>
      </div>
    </div>
  </div>
{/if}
