<template>
  <openwb-base-card
    v-if="$store.state.mqtt[baseTopic + '/module'] && $store.state.mqtt[baseTopic + '/module'].type"
    subtype="secondary"
    :collapsible="true"
    :collapsed="true"
    class="pb-0"
  >
    <template #header>
      <font-awesome-icon :icon="['fas', 'tower-broadcast']" />
      Steuerbare Verbrauchseinrichtung (RSE)
    </template>
    <openwb-base-card
      title="Aktuelle Werte"
      subtype="white"
      body-bg="white"
      class="py-1 mb-2"
    >
      <div class="row">
        <div class="col">Status</div>
        <div
          v-if="$store.state.mqtt[baseTopic + '/get/override_value'] == 0"
          class="col"
        >
          Laden gesperrt
        </div>
        <div
          v-else
          class="col"
        >
          Laden erlaubt ({{ $store.state.mqtt[baseTopic + "/get/override_value"] }}&nbsp;%)
        </div>
      </div>
    </openwb-base-card>
    <template #footer>
      <openwb-base-alert :subtype="getFaultStateSubtype(baseTopic)">
        <font-awesome-icon
          fixed-width
          :icon="stateIcon"
        />
        Modulmeldung:
        <span style="white-space: pre-wrap">{{ $store.state.mqtt[baseTopic + "/get/fault_str"] }}</span>
      </openwb-base-alert>
    </template>
  </openwb-base-card>
</template>

<script>
import ComponentState from "../mixins/ComponentState.vue";

import { library } from "@fortawesome/fontawesome-svg-core";
import {
  faCheckCircle as fasCheckCircle,
  faExclamationTriangle as fasExclamationTriangle,
  faTimesCircle as fasTimesCircle,
  faTowerBroadcast as fasTowerBroadcast,
} from "@fortawesome/free-solid-svg-icons";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

library.add(fasCheckCircle, fasExclamationTriangle, fasTimesCircle, fasTowerBroadcast);

export default {
  name: "RippleControlReceiverCard",
  components: {
    FontAwesomeIcon,
  },
  mixins: [ComponentState],
  data() {
    return {
      mqttTopicsToSubscribe: [
        "openWB/general/ripple_control_receiver/get/fault_state",
        "openWB/general/ripple_control_receiver/get/fault_str",
        "openWB/general/ripple_control_receiver/get/override_value",
        "openWB/general/ripple_control_receiver/module",
      ],
    };
  },
  computed: {
    baseTopic: {
      get() {
        return "openWB/general/ripple_control_receiver";
      },
    },
  },
};
</script>
