<Kitchen bind:this={kitchen} dismiss$class="material-icons" />

<Button on:click={pushToKitchen}><Label>Push a New Snackbar</Label></Button>

<pre class="status">Closed Reason: {reason}</pre>
<pre class="status">Action: {action}</pre>

<script context="module" lang="ts">
  let counter = 0;
</script>

<script lang="ts">
  import type { KitchenComponentDev } from '@smui/snackbar/kitchen';
  import Kitchen from '@smui/snackbar/kitchen';
  import Button, { Label } from '@smui/button';

  let kitchen: KitchenComponentDev;
  let reason = 'nothing yet';
  let action = 'nothing yet';

  function pushToKitchen() {
    kitchen.push({
      props: {
        variant: 'stacked',
      },
      label:
        "This is a snackbar generated by the kitchen. Here's a counter: " +
        counter++,
      actions: [
        {
          onClick: () => (action = 'Something'),
          text: 'Something',
        },
        {
          onClick: () => (action = 'Another'),
          text: 'Another',
        },
      ],
      dismissButton: true,
      onDismiss: () => (action = 'Dismissed'),
      onClose: (e) => {
        reason = e.detail.reason ?? 'Undefined.';
      },
    });
  }
</script>
