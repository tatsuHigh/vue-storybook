<template>
  <p>
    <label :for="name">Email（input[type={{type}}]）</label>
    <input
      :type="type"
      :name="name"
      :value="value"
      :placeholder="placeholder"
      @change="changeValue"
      @blur="changeValue"
      class="input__email"
      :class="classes"
      :id="name"
    />
  </p>
</template>

<script lang="ts">
import { Component, Prop, Vue, Emit } from 'vue-property-decorator';

@Component
export default class InputEmail extends Vue {

  private errors: Array<string> = []

  @Prop()
  private value!: string

  @Prop()
  private type!: string

  @Prop()
  private name!: string

  @Prop({ default: '' })
  private placeholder?: string

  @Prop({ default: false })
  private primary?: boolean

  get classes() {
    return {
      'input__email--primary': this.primary,
      'input__email--secondary': !this.primary,
    };
  }

  private changeValue(event: Event) :void {
    if (event!.target instanceof HTMLInputElement) {
      const e: HTMLInputElement = event.target;
      this.errors = [];

      if (!e.value) {
        this.errors.push("入力が空です。");
      } else if (!this.validEmail(e.value)) {
        this.errors.push("メールアドレスの形式で入力してください。");
      }

      this.updateValue(event!.target)
    }
  }

  private validEmail(email: string) : boolean {
    const re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
    return re.test(email);
  }

  @Emit('input-email')
  private updateValue(elm: HTMLInputElement) :object {
    const value: string = elm.value;
    const errors: Array<string> = this.errors;

    return {
      value,
      errors
    }
  }
}
</script>

<style lang="scss" scorped>
.input__email {
  width: 300px;
  padding: 8px;

  &--primary {
    border: 2px solid rgb(30 167 253);
  }

  &--secondary {
    border: 2px solid #666;
  }
}
</style>