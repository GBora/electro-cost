<script>
  import '../node_modules/water.css/out/dark.min.css';

  let batteryCapacity;
  let costKWH;

  let batteryCapacityKey = 'e-c-bat-cap';
  let costKWHKey = 'e-c-cost-kwh'

  const initValues = () => {
    if (localStorage.getItem(batteryCapacityKey)) {
      batteryCapacity = Number.parseFloat(localStorage.getItem(batteryCapacityKey));
    } else {
      batteryCapacity = 32.3;
    } 

    if (localStorage.getItem(costKWHKey)) {
      costKWH = Number.parseFloat(localStorage.getItem(costKWHKey));
    } else {
      costKWH = 1.97;
    } 
  }

  let batteryChargedPercent = 0;
  let batteryCharged = 0;

  let totalCosts = '';

  const updatePercent = (e) => {
    batteryChargedPercent = e.target.value;
    const kwhCharged = (batteryCapacity * batteryChargedPercent) / 100;
    const costOfCharge = kwhCharged * costKWH;
    totalCosts = `You have to pay ${costOfCharge.toFixed(2)}`
  }

  const updateCharge = (e) => {
    const kwhCharged = e.target.value;
    const costOfCharge = kwhCharged * costKWH;
    totalCosts = `You have to pay ${costOfCharge.toFixed(2)}`
  }

  const updateCapacity = (e) => {
    const capacity = e.target.value;
    batteryCapacity = capacity;
    localStorage.setItem(batteryCapacityKey, capacity);
  }

  const updateCost = (e) => {
    const cost = e.target.value;
    costKWH = cost;
    localStorage.setItem(costKWHKey, cost);
  }

  initValues();
</script>

<main>

  <div class="d-flex">
    <div class="form-label">
      <label for="battery_capacity">Battery Capacity (kWh)</label>
    </div>
    <div>
      <input 
        type="text" 
        class="bat-cap-input" 
        name="battery_capacity" 
        id="battery_capacity"
        value={batteryCapacity}
        on:change={e => updateCapacity(e)}>
    </div>
  </div>

  <div class="d-flex">
    <div class="form-label">
      <label for="cost-kw">Cost (kWh)</label>
    </div>
    <div>
      <input 
        type="text"
        class="bat-cost-input" 
        name="cost-kw" 
        id="cost-kw" 
        value={costKWH}
        on:change={e => updateCost(e)}>
    </div>
  </div>

  <div class="d-flex">
    <div class="form-label-charged">
      <label for="battery_charged_percent">Charged {batteryChargedPercent}% </label>
    </div>
    <div>
      <input 
        type="range"
        min="0"
        max="100"
        step="10" 
        name="battery_charged_percent" 
        id="battery_charged_percent"
        class="slider"
        on:change={e => updatePercent(e)}
        value={batteryChargedPercent}>
    </div>
  </div>

  <div class="d-flex">
    <div class="form-label">
      <label for="battery_charged">Charged (kWh)</label>
    </div>
    <div>
      <input 
        type="number" 
        class="bat-charged-input" 
        name="battery_charged" 
        id="battery_charged"
        min="0"
        max="100"
        on:change={e => updateCharge(e)} 
        value={batteryCharged}>
    </div>
  </div>

  <p>
    {totalCosts}
  </p>

</main>

<style>
  main {
    padding: 2em;
    width: 355px;
    margin: 0 auto;
  }

  .d-flex {
    display: flex;
    flex-direction: row;
    gap: 1em;
  }

  .form-label {
    padding-top: 0.5em;
  }

  .bat-cap-input {
    width: 5em;
  }

  .bat-cost-input {
    width: 5em;
    margin-left: 5.5em;
  }

  .slider {
    width: 11em;
  }

  .form-label-charged {
    padding-top: 0.8em;
  }

  .bat-charged-input {
    width: 5em;
    margin-left: 3.5em;
  }
</style>
