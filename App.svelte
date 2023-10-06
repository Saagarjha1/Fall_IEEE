<svelte:head>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.13.0/css/all.min.css"
      integrity="sha256-h20CPZ0QyXlBuAw7A+KluUYx/3pK+c7lYEpqLTlxjYQ=" crossorigin="anonymous" />
      </svelte:head>

      <script>
        import { onMount } from 'svelte'

          let speed = 1;
            let text = "";
              let textInput;
                let speedInput;
                  let speedDes = 'Speed';
                    let playDes = 'Play';
                      let pauseDes = 'Pause';
                        let stopDes = 'Stop';
                          let currentCharacter;
                            let lang = true;
                              let name = " ";

                                // Define speech synthesis utterance
                                  const utterance = new SpeechSynthesisUtterance();

                                    // Event listener for when speech ends
                                      utterance.addEventListener("end", () => {
                                          textInput.disabled = false;
                                            });

                                              // Event listener for tracking the current character
                                                utterance.addEventListener("boundary", (e) => {
                                                    currentCharacter = e.charIndex;
                                                      });

                                                        // Function to play the entered text
                                                          function playText(text) {
                                                              speedInput.disabled = false;

                                                                  if (speechSynthesis.paused && speechSynthesis.speaking) {
                                                                        return speechSynthesis.resume();
                                                                            }

                                                                                if (speechSynthesis.speaking) return;

                                                                                    utterance.text = text;
                                                                                        utterance.rate = speed || 1;
                                                                                            textInput.disabled = true;
                                                                                                speechSynthesis.speak(utterance);
                                                                                                  }

                                                                                                    // Function to pause speech
                                                                                                      function pause() {
                                                                                                          if (speechSynthesis.speaking) speechSynthesis.pause();
                                                                                                              speedInput.disabled = true;
                                                                                                                }

                                                                                                                  // Function to stop speech
                                                                                                                    function stop() {
                                                                                                                        speechSynthesis.resume();
                                                                                                                            speechSynthesis.cancel();
                                                                                                                                speedInput.disabled = true;
                                                                                                                                  }

                                                                                                                                    // Function to change speech speed
                                                                                                                                      function changeSpeed() {
                                                                                                                                          stop();
                                                                                                                                              playText(utterance.text.substring(currentCharacter));
                                                                                                                                                }

                                                                                                                                                  // Function to switch language
                                                                                                                                                    function switchLang() {
                                                                                                                                                        lang = !lang;
                                                                                                                                                          }
                                                                                                                                                          </script>

                                                                                                                                                          <style>
                                                                                                                                                            .wrap {
                                                                                                                                                                width: 90%;
                                                                                                                                                                    margin: 0 auto;
                                                                                                                                                                        margin-top: 1rem;
                                                                                                                                                                          }

                                                                                                                                                                            .box {
                                                                                                                                                                                display: flex;
                                                                                                                                                                                    justify-content: space-between;
                                                                                                                                                                                      }

                                                                                                                                                                                        .text {
                                                                                                                                                                                            width: 100%;
                                                                                                                                                                                                height: 50vh;
                                                                                                                                                                                                  }

                                                                                                                                                                                                    .right {
                                                                                                                                                                                                        cursor: pointer;
                                                                                                                                                                                                          }
                                                                                                                                                                                                          </style>

                                                                                                                                                                                                          <div class="wrap">
                                                                                                                                                                                                            <textarea class="text" bind:value={text} bind:this={textInput} />
                                                                                                                                                                                                              <div class="box">
                                                                                                                                                                                                                  <div class="left">
                                                                                                                                                                                                                        <label for="speed" style="display: inline-block">
                                                                                                                                                                                                                                {speedDes}
                                                                                                                                                                                                                                        <input
                                                                                                                                                                                                                                                  type="number"
                                                                                                                                                                                                                                                            min=".5"
                                                                                                                                                                                                                                                                      max="3"
                                                                                                                                                                                                                                                                                step=".5"
                                                                                                                                                                                                                                                                                          bind:value={speed}
                                                                                                                                                                                                                                                                                                    on:input={changeSpeed}
                                                                                                                                                                                                                                                                                                              bind:this={speedInput}
                                                                                                                                                                                                                                                                                                                      />
                                                                                                                                                                                                                                                                                                                            </label>
                                                                                                                                                                                                                                                                                                                                  <button on:click={() => playText(text)}>{playDes}</button>
                                                                                                                                                                                                                                                                                                                                        <button on:click={pause}>{pauseDes}</button>
                                                                                                                                                                                                                                                                                                                                              <button on:click={stop}>{stopDes}</button>
                                                                                                                                                                                                                                                                                                                                                  </div>
                                                                                                                                                                                                                                                                                                                                                      <div class="right" on:click={switchLang}>
                                                                                                                                                                                                                                                                                                                                                            <i class="fas fa-globe"></i>
                                                                                                                                                                                                                                                                                                                                                                </div>
                                                                                                                                                                                                                                                                                                                                                                  </div>
                                                                                                                                                                                                                                                                                                                                                                  </div>