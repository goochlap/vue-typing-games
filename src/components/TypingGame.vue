<template>
  <header class="text-center p-3 mb-5">
    <h1>Typing Game</h1>
  </header>
  <div class="container text-center">
    <div class="row">
      <div class="col-md-6 mx-auto">
        <h2 class="display-2 mb-5" id="current-word"></h2>
        <input
          type="text"
          class="form-control form-control-lg"
          placeholder="Start typing..."
          id="word-input"
          autofocus
        />
        <div class="progress">
          <div class="progress-bar" role="progressbar"></div>
        </div>
        <h4 class="mt-3" id="message"></h4>
        <br />
        <button class="btn btn-lg btn-block">START</button>

        <div class="row mt-3">
          <div class="col-md-6">
            <h3>
              Time Left:
              <span id="time"></span>
            </h3>
          </div>
          <div class="col-md-6">
            <h3>
              Score:
              <span id="score">0</span>
            </h3>
          </div>
          <div class="col mt-3 d-flex justify-content-evenly">
            <label for="input-level"><h3>Difficulty:</h3></label>
            <select id="input-level">
              <option>Easy</option>
              <option>Medium</option>
              <option>Hard</option>
            </select>
          </div>
          <div class="col mt-3">
            <h3>
              High Score:
              <span id="highscore">0</span>
            </h3>
          </div>
        </div>

        <div class="row mt-5">
          <div class="col-md-12">
            <div class="card card-body text-white">
              <h5 class="text-warning">Instructions</h5>
              <p>Type as many words as you can until time runs out</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TypingGame'
}
</script>

<style scoped>
header,
.card {
  background: black;
}

.form-control:focus {
  border-color: #e9f897;
  box-shadow: 0 0 0 0.2rem rgba(123, 167, 40, 0.25);
}

.card > p,
header {
  color: white;
}

#message {
  height: 26px;
}

.hidden {
  visibility: hidden;
  opacity: 0;
  transition: visibility 0s 1s, opacity 1s linear;
}

.valid-letter {
  background-color: rgb(223, 223, 55);
  color: rgb(24, 24, 24);
}

.btn,
#input-level {
  background: #f4e623;
}

#input-level {
  border-radius: 0.25rem;
}

.progress {
  margin-top: 22px;
  height: 2rem;
  border-radius: 0.25rem;
}

.progress-bar {
  width: 0%;
  background-color: #e9f897;
  background-image: linear-gradient(to bottom, #e9f897, #f4e623);
}
</style>
