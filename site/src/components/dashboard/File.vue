<template>
  <div class="flex gap-2">

    <!-- Button for exporting as JSON -->
    <UiButton @click="exportToJSON">
      <span i-ic:baseline-save-as size-4 mr-1 />
      {{ $t("dashboard.saveas") }}
    </UiButton>

      <!-- Button for importing as JSON -->
    <UiTooltipProvider :delay-duration="0">
      <UiTooltip>
        <UiTooltipTrigger as-child>
          <UiButton
            class="bg-neutral-800 hover:(bg-neutral-800/90 ring-neutral-800/40) dark:(bg-secondary hover:bg-background hover:ring-secondary/40)"
            @click="open"
          >
            <span i-ic:round-upload-file size-4 mr-1 />
            {{ $t("dashboard.import.title") }}
          </UiButton>
        </UiTooltipTrigger>
        <UiTooltipContent side="bottom" class="w-54 p-0 rounded border-destructive/60">
          <UiAlert variant="destructive" class="border-none rounded-none">
            <UiAlertTitle>
              {{ $t("dashboard.import.alert.title") }}
            </UiAlertTitle>
            <UiAlertDescription v-html="$t('dashboard.import.alert.content')" />
          </UiAlert>
        </UiTooltipContent>
      </UiTooltip>
    </UiTooltipProvider>

  </div>
</template>

<script lang="ts" setup>
import { useShortcuts } from "@ohmycv/vue-shortcuts";
import { useFileDialog, readFile } from "@ohmycv/utils";

const emits = defineEmits<{
  (e: "update"): void;
}>();

const { open, onChange } = useFileDialog(".json");

onChange(async (file) => {
  const content = await readFile(file);
  await storageService.importFromJson(content);
  emits("update");
});

const exportToJSON = () => storageService.exportToJSON();

useShortcuts("shift+ctrl+s", exportToJSON);
</script>
