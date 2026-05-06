<script lang="ts">
  type Props = {
    remainingCount: number;
    completedCount: number;
    onClearCompleted: () => void;
  };

  let { remainingCount, completedCount, onClearCompleted }: Props = $props();
</script>

<div class="w-full max-w-md flex items-center justify-between mt-4 px-1 text-sm text-gray-600">
  <span>残り {remainingCount} 件 / 完了 {completedCount} 件</span>
  <button
    onclick={onClearCompleted}
    disabled={completedCount === 0}
    class="text-gray-500 hover:text-red-500 hover:underline disabled:text-gray-300 disabled:no-underline disabled:cursor-not-allowed"
  >
    完了したタスクを削除
  </button>
</div>
