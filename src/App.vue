<script setup lang="ts">
import {computed, reactive} from "vue";

interface Planet {
    name: string;
    oxygenPercentage: number;
}

const state = reactive({
    name: "",
    showForm: false,
})

const planets: Array<Planet> = reactive([
    {
        name: "🌎 Earth",
        oxygenPercentage: 21,
    },
    {
        name: "🪐 Saturn",
        oxygenPercentage: 0.01,
    },
])

const editedPlanet: Planet = reactive({
    name: "",
    oxygenPercentage: 0,
})

const planetCount = computed<number>(() => planets.length);
const percentageOfHabitablePlanets = computed<number>(() => {
    if (planets.length === 0) {
        return 0;
    }
    const habitablePlanets = planets.filter(planet => planet.oxygenPercentage >= 19 && planet.oxygenPercentage <= 24);
    return (habitablePlanets.length / planets.length) * 100;
});

const savePlanet = () => {
    // add a copy to the planets array (do not mutate the original because it is used in the form)
    const copy = {...editedPlanet};
    planets.push(copy);

    // reset the editedPlanet to default values and hide the form
    editedPlanet.name = "";
    editedPlanet.oxygenPercentage = 0;
    state.showForm = false;
}

const removePlanet = (planet: Planet) => {
    const index = planets.indexOf(planet);
    planets.splice(index, 1);
}
</script>

<template>
    <h1>Hello {{ state.name }}!</h1>
    Your name: <input v-model="state.name" />
    <br>
    <br>

    <h2>Planets</h2>
    <p>
        <i v-if="planetCount === 0">
            We have discovered no planets yet.
        </i>
        <i v-else>
          We have discovered {{ planetCount }} planets so far.
        </i>
    </p>
    <p>
        <i v-if="percentageOfHabitablePlanets === 0">
            None of these planets are habitable.
        </i>
        <i v-else>
            Hooray! 🥳
            {{ percentageOfHabitablePlanets.toFixed(1) }}% of these planets are habitable. Let's hop into the rocket! 🚀
        </i>
    </p>
    <ul>
        <li v-for="planet in planets" :key="planet.name">
            <strong>{{ planet.name }}</strong> with {{ planet.oxygenPercentage }}% oxygen
            <a href="#" @click="removePlanet(planet)">Remove</a>
        </li>
    </ul>

    <button v-if="!state.showForm" @click="state.showForm = true">Add planet</button>
    <div v-else>
        <h4>Add planet</h4>
        <label>
            Name:
            <input v-model="editedPlanet.name" />
        </label>
        <br>
        <label>
            Oxygen percentage:
            <input v-model.number="editedPlanet.oxygenPercentage" />
        </label>
        <br>
        <button @click="savePlanet()">Save</button>
    </div>

</template>

<style scoped>
</style>
