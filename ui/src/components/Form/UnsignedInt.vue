<template>
  <v-text-field
    v-model="unsignedInt"
    :label="label"
    v-if="parseInt(max) <= 1"
    outline
    :required="required"
    :rules="[rules.inRange, rules.integer, rules.required]"
    :value="value"
    :hint="hint"
  ></v-text-field>
  <v-combobox
    v-else
    v-model="unsignedInt"
    hide-selected
    :label="label"
    multiple
    persistent-hint
    small-chips
    :rules="[rules.inRange, rules.integer, rules.max, rules.required]"
    :required="required"
    outline
    :hint="hint"
  ></v-combobox>
</template>

<script>
export default {
  created() {
    this.unsignedInt = this.value;
  },
  data() {
    return {
      rules: {
        inRange: value => {
          if (!Array.isArray(value)) {
            value = [value];
          }

          for (var i = 0; i < value.length; i++) {
            if (value[i] < 0) {
              return "Value must be greater than 0.";
            }

            if (value[i] > 2147483647) {
              return "Value must be less than 2147483647.";
            }
          }

          return true;
        },
        integer: value => {
          if (!Array.isArray(value)) {
            value = [value];
          }

          for (var i = 0; i < value.length; i++) {
            if (!Number.isInteger(value[i])) {
              return "Value must be an integer";
            }
          }

          return true;
        },
        max: value => {
          if (this.max == "*") {
            return true;
          }

          let max = parseInt(this.max);

          if (value.length > max) {
            return "Only " + max + " entries allowed.";
          }

          return true;
        },
        required: value => {
          return (
            (this.required && value) || !this.required || "Field is required"
          );
        }
      },
      unsignedInt: null
    };
  },
  methods: {
    getInput() {
      return this["unsignedInt"];
    }
  },
  props: ["label", "max", "required", "value", "hint"]
};
</script>
