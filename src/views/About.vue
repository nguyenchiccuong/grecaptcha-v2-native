<template>
  <div class="about">
    <h1>This is an about page</h1>
  </div>
  <GReCaptchaV2
    :captchaLanguage="captchaLanguage"
    :captchaSitekey="captchaSitekey"
    :captchaTheme="captchaTheme"
    :captchaSize="captchaSize"
    :captchaTabindex="captchaTabindex"
    @captchaLoadedSuccess="captchaLoadedSuccess"
    @captchaVerifiedSuccessCallback="captchaVerifiedSuccessCallback"
    @captchaExpiredCallback="captchaExpiredCallback"
    @captchaErrorCallback="captchaErrorCallback"
  />
</template>

<script lang="ts">
import { Options, Vue } from "vue-class-component";
import GReCaptchaV2 from "@/components/GReCaptchaV2.vue";

@Options({
  components: {
    GReCaptchaV2,
  },
})
export default class About extends Vue {
  captchaLanguage?: string = "fr";
  captchaSitekey: string = "6LeQu8cfAAAAAB7rJlm01UpiXM_kDDGof005gRuq";
  captchaTheme?: string = "light";
  captchaSize?: string = "normal";
  captchaTabindex?: number = 0;
  captchaWidgetId?: string;

  captchaLoadedSuccess(gReCaptchaWidgetId: string): void {
    this.captchaWidgetId = gReCaptchaWidgetId;
    console.log(
      `Captcha loaded success with widget id: ${this.captchaWidgetId}`
    );
  }

  captchaVerifiedSuccessCallback(response: string): void {
    console.log(`Response from captcha: ${response}`);
  }

  captchaExpiredCallback(): void {
    console.log(`Captcha expired`);
  }

  captchaErrorCallback(): void {
    console.log(`Captcha error`);
  }
}
</script>
