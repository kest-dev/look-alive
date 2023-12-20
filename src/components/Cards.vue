<script setup lang="ts">
import { ref } from 'vue'
import { reactive } from "@vue/reactivity";

let title = "";
let desc = "";
let image = "";

class Card {
  public image: string;
  public title: string;
  public desc: string;
  private privDesc: string;
  private isDone: boolean;
  constructor(image: string, title: string, desc: string) {
    this.image = image;
    this.title = title;
    this.desc = desc;
    this.privDesc = "";
    this.isDone = false;
  }
  
  // Checks if the to-do is finished.
  public IsDone()
  {
    return this.isDone;
  }

  // Toggles the done status.
  public ToggleDone()
  {
    this.isDone = !this.isDone;
  }

  // Show or hide the description if needed.
  public EnableDescription()
  {
    if(this.desc === "")
    {
      this.desc = this.privDesc;
    }
    else
    {
      this.privDesc = this.desc;
      this.desc = "";
    }
  }
}

// Success class references
const success = ref('card-body text-success');
const successborder = ref('border-success');

// Regular card class
const regular = ref('card-body');

let cards = reactive([] as Array<Card>);

// Is the title a duplicate of another card?
function IsDuplicate(title: string) {
  for (let card of cards) {
    if (card.title === title) {
      return true;
    }
  }

  return false;
}

// Create a new card.
function CreateCard(image: string, title: string, desc: string) {
  if (!IsDuplicate(title)) {
    let newCard = new Card(image, title, desc);
    cards.push(newCard);
  }
}
</script>

<template>
  <div class="input-group mb-3">
    <span class="input-group-text" id="basic-addon1">To-Do: </span>
    <input
      type="text"
      class="form-control"
      v-model="title"
      placeholder="Title: Getting Groceries..."
      aria-label="Username"
      aria-describedby="basic-addon1"
    />
    <input
      type="text"
      class="form-control"
      v-model="desc"
      placeholder="Description: Get milk..."
      aria-label="Username"
      aria-describedby="basic-addon1"
    />
    <button type="button" class="btn" @click="CreateCard(image, title, desc)">
      Add Card
    </button>
  </div>
  <div class="container">
    <div class="card-group">
      <div v-for="(card, index) of cards" :key="index">
        <div class="card " :class="card.IsDone() ? successborder : ''" style="width: 18rem">
          <div :class="[card.IsDone() ? success : regular]">
            <h5 class="card-title">{{ card.title }}</h5>
            <p class="card-text">{{ card.desc }}</p>
            <button :disabled="card.IsDone()" @click="card.ToggleDone()" class="btn btn-primary btn-sm">Mark as done</button>
            <button @click="card.EnableDescription()" class="btn btn-primary btn-sm m-2">Toggle Description</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
