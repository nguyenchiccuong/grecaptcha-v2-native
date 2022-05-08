<template>
  <div id="g-recaptcha" class="g-recaptcha"></div>
</template>

<script lang="ts">
import { Vue, prop } from "vue-class-component";

export default class GReCaptchaV2 extends Vue.with(
  class {
    captchaLanguage = prop<string>({ default: "en" });
    captchaSitekey = prop<string>({ default: "" });
    captchaTheme = prop<string>({ default: "light" });
    captchaSize = prop<string>({ default: "normal" });
    captchaTabindex = prop<number>({ default: 0 });
  }
) {
  window!: any;

  beforeMount() {
    this.window = window;
    this.window.captchaLoadedSuccess = this.captchaLoadedSuccess;
  }

  mounted() {
    let script = document.createElement("script");
    script.setAttribute(
      "src",
      `https://www.google.com/recaptcha/api.js?onload=captchaLoadedSuccess&render=explicit&hl=${this.captchaLanguage}`
    );
    script.async = true;
    script.defer = true;
    document.head.appendChild(script);
  }

  captchaLoadedSuccess(): void {
    const gReCaptchaWidgetId = this.window.grecaptcha.render("g-recaptcha", {
      sitekey: this.captchaSitekey,
      theme: this.captchaTheme,
      size: this.captchaSize,
      tabindex: this.captchaTabindex,
      callback: this.captchaVerifiedSuccessCallback,
      "expired-callback": this.captchaExpiredCallback,
      "error-callback": this.captchaErrorCallback,
    });
    this.$emit("captchaLoadedSuccess", gReCaptchaWidgetId);
  }

  captchaVerifiedSuccessCallback(response: string): void {
    this.$emit("captchaVerifiedSuccessCallback", response);
  }

  captchaExpiredCallback(): void {
    this.$emit("captchaExpiredCallback");
  }

  captchaErrorCallback(): void {
    this.$emit("captchaErrorCallback");
  }
}
</script>

<style scoped>
</style>