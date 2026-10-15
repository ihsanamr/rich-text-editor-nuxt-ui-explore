<script setup lang="ts">
// Demo <UEditor> Nuxt UI: markdown, text-align, highlight, upload gambar lokal,slash command, dan drag-and-drop blok.
import type { Editor } from "@tiptap/vue-3";
import { TextAlign } from "@tiptap/extension-text-align";
import { Highlight } from "@tiptap/extension-highlight";
import type {
  EditorCustomHandlers,
  EditorSuggestionMenuItem,
  EditorToolbarItem,
} from "@nuxt/ui";
import { ImageUpload } from "~/components/EditorImageUploadExtension";

const content = ref(`# Nuxt UI Editor

This page is an exploration of the **Editor** component from [Nuxt UI](https://ui.nuxt.com/docs/components/editor), which is built on top of [TipTap](https://tiptap.dev/).

## Try it out

Select some text to reveal the toolbar actions, hover the left edge of a block to drag and reorder it, or type \`/\` on an empty line to open the slash command menu.

> The editor below is bound with \`v-model\` and uses the \`markdown\` content type.
`);

const extensions = [
  TextAlign.configure({ types: ["heading", "paragraph"] }),
  Highlight,
  ImageUpload,
];

// Handler untuk fitur yang tidak disediakan UEditor secara bawaan.
const customHandlers = {
  highlight: {
    canExecute: (editor: Editor) => editor.can().toggleHighlight(),
    execute: (editor: Editor) => editor.chain().focus().toggleHighlight(),
    isActive: (editor: Editor) => editor.isActive("highlight"),
    isDisabled: (editor: Editor) => !editor.isEditable,
  },
  imageUpload: {
    canExecute: (editor: Editor) =>
      editor.can().insertContent({ type: "imageUpload" }),
    execute: (editor: Editor) =>
      editor.chain().focus().insertContent({ type: "imageUpload" }),
    isActive: (editor: Editor) => editor.isActive("imageUpload"),
  },
} satisfies EditorCustomHandlers;

// Array-of-array: array luar = grup tombol, array dalam = tombol per grup.
const toolbarItems: EditorToolbarItem<typeof customHandlers>[][] = [
  [
    { kind: "undo", icon: "i-lucide-undo", tooltip: { text: "Undo" } },
    { kind: "redo", icon: "i-lucide-redo", tooltip: { text: "Redo" } },
  ],
  [
    {
      icon: "i-lucide-heading",
      tooltip: { text: "Headings" },
      content: { align: "start" },
      items: [
        {
          kind: "heading",
          level: 1,
          icon: "i-lucide-heading-1",
          label: "Heading 1",
        },
        {
          kind: "heading",
          level: 2,
          icon: "i-lucide-heading-2",
          label: "Heading 2",
        },
        {
          kind: "heading",
          level: 3,
          icon: "i-lucide-heading-3",
          label: "Heading 3",
        },
        {
          kind: "heading",
          level: 4,
          icon: "i-lucide-heading-4",
          label: "Heading 4",
        },
      ],
    },
    {
      icon: "i-lucide-list",
      tooltip: { text: "Lists" },
      content: { align: "start" },
      items: [
        { kind: "bulletList", icon: "i-lucide-list", label: "Bullet List" },
        {
          kind: "orderedList",
          icon: "i-lucide-list-ordered",
          label: "Ordered List",
        },
      ],
    },
    {
      kind: "blockquote",
      icon: "i-lucide-text-quote",
      tooltip: { text: "Blockquote" },
    },
    {
      kind: "codeBlock",
      icon: "i-lucide-square-code",
      tooltip: { text: "Code Block" },
    },
    {
      kind: "horizontalRule",
      icon: "i-lucide-separator-horizontal",
      tooltip: { text: "Divider" },
    },
  ],
  [
    {
      kind: "mark",
      mark: "bold",
      icon: "i-lucide-bold",
      tooltip: { text: "Bold" },
    },
    {
      kind: "mark",
      mark: "italic",
      icon: "i-lucide-italic",
      tooltip: { text: "Italic" },
    },
    {
      kind: "mark",
      mark: "underline",
      icon: "i-lucide-underline",
      tooltip: { text: "Underline" },
    },
    {
      kind: "mark",
      mark: "strike",
      icon: "i-lucide-strikethrough",
      tooltip: { text: "Strikethrough" },
    },
    {
      kind: "mark",
      mark: "code",
      icon: "i-lucide-code",
      tooltip: { text: "Code" },
    },
    {
      kind: "highlight",
      icon: "i-lucide-highlighter",
      tooltip: { text: "Highlight" },
    },
  ],
  [
    {
      icon: "i-lucide-align-justify",
      tooltip: { text: "Text Align" },
      content: { align: "end" },
      items: [
        {
          kind: "textAlign",
          align: "left",
          icon: "i-lucide-align-left",
          label: "Align Left",
        },
        {
          kind: "textAlign",
          align: "center",
          icon: "i-lucide-align-center",
          label: "Align Center",
        },
        {
          kind: "textAlign",
          align: "right",
          icon: "i-lucide-align-right",
          label: "Align Right",
        },
        {
          kind: "textAlign",
          align: "justify",
          icon: "i-lucide-align-justify",
          label: "Align Justify",
        },
      ],
    },
  ],
  [
    { kind: "link", icon: "i-lucide-link", tooltip: { text: "Link" } },
    {
      kind: "image",
      icon: "i-lucide-image",
      tooltip: { text: "Image from URL" },
    },
    {
      kind: "imageUpload",
      icon: "i-lucide-image-plus",
      label: "Upload",
      tooltip: { text: "Upload image" },
    },
  ],
];

// Item menu slash command; `type: 'label'` jadi judul seksi.
const suggestionItems: EditorSuggestionMenuItem[][] = [
  [
    { type: "label", label: "Text" },
    { kind: "paragraph", label: "Paragraph", icon: "i-lucide-type" },
    {
      kind: "heading",
      level: 1,
      label: "Heading 1",
      icon: "i-lucide-heading-1",
    },
    {
      kind: "heading",
      level: 2,
      label: "Heading 2",
      icon: "i-lucide-heading-2",
    },
    {
      kind: "heading",
      level: 3,
      label: "Heading 3",
      icon: "i-lucide-heading-3",
    },
  ],
  [
    { type: "label", label: "Lists" },
    { kind: "bulletList", label: "Bullet List", icon: "i-lucide-list" },
    {
      kind: "orderedList",
      label: "Numbered List",
      icon: "i-lucide-list-ordered",
    },
  ],
  [
    { type: "label", label: "Insert" },
    { kind: "blockquote", label: "Blockquote", icon: "i-lucide-text-quote" },
    { kind: "codeBlock", label: "Code Block", icon: "i-lucide-square-code" },
    {
      kind: "horizontalRule",
      label: "Divider",
      icon: "i-lucide-separator-horizontal",
    },
    { kind: "imageUpload", label: "Upload image", icon: "i-lucide-image-plus" },
  ],
];

const toast = useToast();

async function copyMarkdown() {
  await navigator.clipboard.writeText(content.value);
  toast.add({
    title: "Markdown copied",
    icon: "i-lucide-clipboard-check",
    color: "success",
  });
}
</script>

<template>
  <div class="min-h-screen bg-default">
    <div class="mx-auto max-w-3xl px-4 py-10 space-y-6">
      <header class="space-y-1">
        <h1 class="text-2xl font-bold text-highlighted">Nuxt UI Editor</h1>
        <p class="text-muted">
          A rich text editor built on TipTap with markdown support, text
          alignment, highlight, local image upload, slash commands and
          drag-and-drop blocks.
        </p>
      </header>

      <UCard :ui="{ body: 'p-0 sm:p-0' }">
        <UEditor
          v-slot="{ editor }"
          v-model="content"
          content-type="markdown"
          :extensions="extensions"
          :handlers="customHandlers"
          placeholder="Type / for commands..."
          class="w-full min-h-96 flex flex-col"
        >
          <UEditorToolbar
            :editor="editor"
            :items="toolbarItems"
            layout="fixed"
            class="border-b border-default px-2 py-2 overflow-x-auto"
          />

          <UEditorSuggestionMenu :editor="editor" :items="suggestionItems" />

          <UEditorDragHandle :editor="editor" />
        </UEditor>
      </UCard>

      <!-- Menampilkan serialisasi markdown dari v-model secara live. -->
      <UCard>
        <template #header>
          <div class="flex items-center justify-between">
            <span class="font-medium text-highlighted">Markdown output</span>
            <UButton
              icon="i-lucide-clipboard"
              label="Copy"
              color="neutral"
              variant="soft"
              size="xs"
              @click="copyMarkdown"
            />
          </div>
        </template>
        <pre
          class="overflow-x-auto text-sm text-muted whitespace-pre-wrap wrap-break-word"
          >{{ content }}</pre
        >
      </UCard>
    </div>
  </div>
</template>
