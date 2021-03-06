<template>
  <div class="capability-type-data">

    <app-labeled-input
      :formstate="formstate"
      :name="`capability${capability.uuid}-shutterEffect`"
      label="Shutter effect">
      <select
        v-model="capability.typeData.shutterEffect"
        :class="{ empty: capability.typeData.shutterEffect === `` }"
        :name="`capability${capability.uuid}-shutterEffect`"
        required>

        <option value="" disabled>Please select a shutter effect</option>
        <option
          v-for="effect in shutterEffects"
          :key="effect"
          :value="effect">{{ effect }}</option>

      </select>
    </app-labeled-input>

    <template v-if="isStrobeEffect">
      <app-labeled-input
        :formstate="formstate"
        :name="`capability${capability.uuid}-soundControlled`"
        label="Sound controlled?">
        <app-property-input-boolean
          v-model="capability.typeData.soundControlled"
          :schema-property="properties.capabilityTypes.ShutterStrobe.properties.soundControlled"
          :name="`capability${capability.uuid}-soundControlled`" />
      </app-labeled-input>

      <app-labeled-input
        :formstate="formstate"
        :name="`capability${capability.uuid}-speed`"
        label="Speed">
        <app-editor-proportional-capability-data-switcher
          :capability="capability"
          :formstate="formstate"
          property-name="speed"
          entity="speed" />
      </app-labeled-input>

      <app-labeled-input
        :formstate="formstate"
        :name="`capability${capability.uuid}-duration`"
        label="Duration">
        <app-editor-proportional-capability-data-switcher
          :capability="capability"
          :formstate="formstate"
          property-name="duration" />
      </app-labeled-input>

      <app-labeled-input
        :formstate="formstate"
        :name="`capability${capability.uuid}-randomTiming`"
        label="Random timing?">
        <app-property-input-boolean
          v-model="capability.typeData.randomTiming"
          :schema-property="properties.capabilityTypes.ShutterStrobe.properties.randomTiming"
          :name="`capability${capability.uuid}-randomTiming`" />
      </app-labeled-input>
    </template>

    <app-labeled-input
      :formstate="formstate"
      :name="`capability${capability.uuid}-comment`"
      label="Comment">
      <app-property-input-text
        v-model="capability.typeData.comment"
        :formstate="formstate"
        :name="`capability${capability.uuid}-comment`"
        :schema-property="properties.definitions.nonEmptyString" />
    </app-labeled-input>

  </div>
</template>

<script>
import schemaProperties from '~~/lib/schema-properties.js';

import editorProportionalCapabilityDataSwitcher from '~/components/editor-proportional-capability-data-switcher.vue';
import propertyInputBooleanVue from '~/components/property-input-boolean.vue';
import propertyInputTextVue from '~/components/property-input-text.vue';
import labeledInputVue from '~/components/labeled-input.vue';

export default {
  components: {
    'app-editor-proportional-capability-data-switcher': editorProportionalCapabilityDataSwitcher,
    'app-property-input-boolean': propertyInputBooleanVue,
    'app-property-input-text': propertyInputTextVue,
    'app-labeled-input': labeledInputVue
  },
  props: {
    capability: {
      type: Object,
      required: true
    },
    formstate: {
      type: Object,
      required: false,
      default: null
    }
  },
  data() {
    return {
      properties: schemaProperties,
      defaultData: {
        shutterEffect: ``,
        soundControlled: null,
        speed: null,
        speedStart: ``,
        speedEnd: ``,
        duration: ``,
        durationStart: null,
        durationEnd: null,
        randomTiming: null,
        comment: ``
      }
    };
  },
  computed: {
    shutterEffects() {
      return this.properties.capabilityTypes.ShutterStrobe.properties.shutterEffect.enum;
    },
    isStrobeEffect() {
      return ![``, `Open`, `Closed`].includes(this.capability.typeData.shutterEffect);
    },
    resetProps() {
      if (!this.isStrobeEffect) {
        return [
          `soundControlled`,
          `speed`,
          `speedStart`,
          `speedEnd`,
          `duration`,
          `durationStart`,
          `durationEnd`,
          `randomTiming`
        ];
      }

      return [];
    }
  }
};
</script>
