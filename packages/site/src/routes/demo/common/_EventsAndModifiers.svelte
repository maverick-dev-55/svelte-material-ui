<div>
  <Button
    on:click={addEvent}
    on:mousedown={addEvent}
    on:mouseup={addEvent}
    on:mouseover={addEvent}
    on:mouseout={addEvent}
    on:keypress={addEvent}
    on:keydown={addEvent}
    on:keyup={addEvent}
    on:focus={addEvent}
    on:blur={addEvent}
    on:animationstart={addEvent}
    on:animationend={addEvent}
  >
    <Label>This Button has Event Listeners</Label>
  </Button>
</div>

<div class="event-output" bind:this={eventOutput}>
  {#each events as event}
    <p>Caught {event.type}</p>
  {:else}
    <p>No events yet.</p>
  {/each}
</div>

<div>
  Try clicking and using the Enter key to activate this next button. (The click
  event's target is an element below the button while the key event's target is
  the button.)
</div>

<div>
  <Button on:click$capture={addEventPhase} on:click={addEventPhase}>
    <Label>Capture and Bubble Phase Listeners</Label>
  </Button>
</div>

<div class="event-output" bind:this={eventPhaseOutput}>
  {#each eventPhases as event}
    <p>Caught {event[0].type} in {event[1]} phase</p>
  {:else}
    <p>No events yet.</p>
  {/each}
</div>

<br /> <br />

<div>
  <A
    href="http://example.com"
    on:click$preventDefault={() =>
      console.log("You tried to go, but didn't make it.")}
  >
    A Link, with Default Prevented
  </A>
</div>

<br /> <br />

<div>
  <Button on:mousedown$preventDefault={handleTypedEvent}>
    Correct Event Type
  </Button>
</div>

<pre class="status">Mouse Button: {button}</pre>

<script lang="ts">
  import Button, { Label } from '@smui/button';
  import { A } from '@smui/common/elements';

  let eventOutput: HTMLDivElement;
  let eventPhaseOutput: HTMLDivElement;
  let events: Event[] = [];
  let eventPhases: [Event, string][] = [];

  function addEvent(event: Event) {
    events.push(event);
    events = events;
    requestAnimationFrame(() => {
      eventOutput.scrollTop = eventOutput.scrollHeight;
    });
  }

  function addEventPhase(event: Event) {
    const phases = ['none', 'capturing', 'at-target', 'bubbling'];
    eventPhases.push([event, phases[event.eventPhase]]);
    eventPhases = eventPhases;
    requestAnimationFrame(() => {
      eventPhaseOutput.scrollTop = eventPhaseOutput.scrollHeight;
    });
  }

  let button = 'None yet.';

  // In order to get the correct type, you need to reassign the
  // event with a type assertion.
  function handleTypedEvent(event: CustomEvent | MouseEvent) {
    event = event as MouseEvent;
    button =
      ['Left.', 'Middle.', 'Right.'][event.button] ??
      `Button #${event.button}.`;
  }
</script>

<style>
  .event-output {
    font-family: 'Roboto Mono', monospace !important;
    height: 200px;
    overflow: auto;
  }
</style>
