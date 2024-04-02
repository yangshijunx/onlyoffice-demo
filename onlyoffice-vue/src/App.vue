<template>
  <DocumentEditor
    id="docEditor"
    :documentServerUrl="'http://192.168.0.101:9091/'"
    :config="config"
    :events_onDocumentReady="onDocumentReady"
  />
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { DocumentEditor } from "@onlyoffice/document-editor-vue";

export default defineComponent({
  name: "ExampleComponent",
  components: {
    DocumentEditor,
  },
  data() {
    return {
      config: {
        document: {
          permissions: {
            chat: true,
            comment: true,
            copy: true,
            commentGroups: {
              edit: ["Group2", ""],
              remove: [""],
              view: "",
            },
            deleteCommentAuthorOnly: false,
            download: true,
            edit: true,
            editCommentAuthorOnly: false,
            fillForms: true,
            modifyContentControl: true,
            print: true,
            protect: true,
            review: false,
            reviewGroups: ["Group1", "Group2", ""],
            userInfoGroups: ["Group1", ""],
          },
          fileType: "docx",
          key: "Khirz6zTPdfd7",
          title: "Example Document Title.docx",
          url: "https://nuannuan.tos-cn-beijing.volces.com/h5/video/%E6%B5%8B%E8%AF%95.docx",
        },
        documentType: "word",
        editorConfig: {
          lang: "zh", // 设置语言
          callbackUrl: "http://192.168.0.101:9091/url-to-callback.ashx",
        },
        // token: "1234567890",
        token:
          "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJkb2N1bWVudCI6eyJkaXJlY3RVcmwiOiIiLCJmaWxlVHlwZSI6ImRvY3giLCJpbmZvIjp7Im93bmVyIjoiTWUiLCJ1cGxvYWRlZCI6IlR1ZSBBcHIgMDIgMjAyNCIsImZhdm9yaXRlIjpudWxsfSwia2V5IjoiX19mZmZmXzE5Mi4xNjguMC4xMDFuZXcuZG9jeDExNzExOTg3MTE5MTIwIiwicGVybWlzc2lvbnMiOnsiY2hhdCI6dHJ1ZSwiY29tbWVudCI6dHJ1ZSwiY29weSI6dHJ1ZSwiZG93bmxvYWQiOnRydWUsImVkaXQiOnRydWUsImZpbGxGb3JtcyI6dHJ1ZSwibW9kaWZ5Q29udGVudENvbnRyb2wiOnRydWUsIm1vZGlmeUZpbHRlciI6dHJ1ZSwicHJpbnQiOnRydWUsInJldmlldyI6dHJ1ZSwicmV2aWV3R3JvdXBzIjpudWxsLCJjb21tZW50R3JvdXBzIjp7fSwidXNlckluZm9Hcm91cHMiOm51bGwsInByb3RlY3QiOnRydWV9LCJyZWZlcmVuY2VEYXRhIjp7ImZpbGVLZXkiOiJ7XCJmaWxlTmFtZVwiOlwibmV3LmRvY3hcIixcInVzZXJBZGRyZXNzXCI6XCJfX2ZmZmZfMTkyLjE2OC4wLjEwMVwifSIsImluc3RhbmNlSWQiOiJodHRwOi8vMTkyLjE2OC4wLjEwMTozMDAwIn0sInRpdGxlIjoibmV3LmRvY3giLCJ1cmwiOiJodHRwOi8vMTkyLjE2OC4wLjEwMTozMDAwL2Rvd25sb2FkP2ZpbGVOYW1lPW5ldy5kb2N4JnVzZXJhZGRyZXNzPV9fZmZmZl8xOTIuMTY4LjAuMTAxIn0sImRvY3VtZW50VHlwZSI6IndvcmQiLCJlZGl0b3JDb25maWciOnsiYWN0aW9uTGluayI6bnVsbCwiY2FsbGJhY2tVcmwiOiJodHRwOi8vMTkyLjE2OC4wLjEwMTozMDAwL3RyYWNrP2ZpbGVuYW1lPW5ldy5kb2N4JnVzZXJhZGRyZXNzPV9fZmZmZl8xOTIuMTY4LjAuMTAxIiwiY29FZGl0aW5nIjpudWxsLCJjcmVhdGVVcmwiOiJodHRwOi8vMTkyLjE2OC4wLjEwMTozMDAwL2VkaXRvcj9maWxlRXh0PWRvY3gmdXNlcmlkPXVpZC0xJnR5cGU9ZGVza3RvcCZsYW5nPXpoIiwiY3VzdG9taXphdGlvbiI6eyJhYm91dCI6dHJ1ZSwiY29tbWVudHMiOnRydWUsImZlZWRiYWNrIjp0cnVlLCJmb3JjZXNhdmUiOmZhbHNlLCJnb2JhY2siOnsidXJsIjoiaHR0cDovLzE5Mi4xNjguMC4xMDE6MzAwMC8ifSwic3VibWl0Rm9ybSI6ZmFsc2V9LCJlbWJlZGRlZCI6eyJlbWJlZFVybCI6Imh0dHA6Ly8xOTIuMTY4LjAuMTAxOjMwMDAvZG93bmxvYWQ_ZmlsZU5hbWU9bmV3LmRvY3giLCJzYXZlVXJsIjoiaHR0cDovLzE5Mi4xNjguMC4xMDE6MzAwMC9kb3dubG9hZD9maWxlTmFtZT1uZXcuZG9jeCIsInNoYXJlVXJsIjoiaHR0cDovLzE5Mi4xNjguMC4xMDE6MzAwMC9kb3dubG9hZD9maWxlTmFtZT1uZXcuZG9jeCIsInRvb2xiYXJEb2NrZWQiOiJ0b3AifSwiZmlsZUNob2ljZVVybCI6IiIsImxhbmciOiJ6aCIsIm1vZGUiOiJlZGl0IiwicGx1Z2lucyI6eyJwbHVnaW5zRGF0YSI6W119LCJ0ZW1wbGF0ZXMiOlt7ImltYWdlIjoiIiwidGl0bGUiOiJCbGFuayIsInVybCI6Imh0dHA6Ly8xOTIuMTY4LjAuMTAxOjMwMDAvZWRpdG9yP2ZpbGVFeHQ9ZG9jeCZ1c2VyaWQ9dWlkLTEmdHlwZT1kZXNrdG9wJmxhbmc9emgifSx7ImltYWdlIjoiaHR0cDovLzE5Mi4xNjguMC4xMDE6MzAwMC9pbWFnZXMvZmlsZV9kb2N4LnN2ZyIsInRpdGxlIjoiV2l0aCBzYW1wbGUgY29udGVudCIsInVybCI6Imh0dHA6Ly8xOTIuMTY4LjAuMTAxOjMwMDAvZWRpdG9yP2ZpbGVFeHQ9ZG9jeCZ1c2VyaWQ9dWlkLTEmdHlwZT1kZXNrdG9wJmxhbmc9emgmc2FtcGxlPXRydWUifV0sInVzZXIiOnsiZ3JvdXAiOiIiLCJpZCI6InVpZC0xIiwibmFtZSI6IkpvaG4gU21pdGgifX0sImhlaWdodCI6IjEwMCUiLCJ0b2tlbiI6IiIsInR5cGUiOiJkZXNrdG9wIiwid2lkdGgiOiIxMDAlIiwiaWF0IjoxNzEyMDIyODc2LCJleHAiOjE3MTIwMjMxNzZ9.4COXFCTBEtQRQQbP2JXI6_tI7vHTI7OAZfQxa9flYl0",
      },
    };
  },
  created() {
    this.getConfig();
  },
  methods: {
    getConfig() {
      // 使用fetch get请求 http://127.0.0.1:3000/editor-config?fileName=new.docx&lang=zh&directUrl=false
      // https://api.onlyoffice.com/editors/config/document/permissions 文档地址
      // 后端接口需要使用局域网ip docker -> node -> vue
      fetch(`http://192.168.0.101:3000/editor-config?fileName=new.docx&lang=zh
      &directUrl=false`)
        .then((response) => response.json())
        .then((data) => {
          // console.log("Config data:", data);
          this.config = data;
          this.config.editorConfig.lang = "zh";
        })
        .catch((error) => {
          console.error("Error fetching config:", error);
        });
    },
    onDocumentReady() {
      console.log("Document is loaded", this.config);
    },
  },
});
</script>
<style>
#app {
  width: 100vw;
  height: 100vh;
  margin: 0;
  padding: 0;
}
iframe {
  width: 100vw;
}
</style>
