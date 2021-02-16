<template>
    <div class="container">
        <header>
            <div class="header-logo">
                <img src="../assets/logo.svg" alt="logo" />
            </div>
            <div class="score-wrapper">
                <h2>Score</h2>
                <span>{{ score }}</span>
            </div>
        </header>

        <transition name="opacity-main">
        <main v-if="firststep">
            <button @click="humanChoice('paper')" class="circle-btn btn-paper">
                <div class="wrapper-img">
                    <img src="../assets/icon-paper.svg" alt="paper icon" />
                </div>
            </button>

            <button
                @click="humanChoice('scissors')"
                class="circle-btn btn-scissors"
            >
                <div class="wrapper-img">
                    <img
                        src="../assets/icon-scissors.svg"
                        alt="scissors icon"
                    />
                </div>
            </button>

            <button @click="humanChoice('rock')" class="circle-btn btn-rock">
                <div class="wrapper-img">
                    <img src="../assets/icon-rock.svg" alt="rock icon" />
                </div>
            </button>
        </main>
        </transition>

        <section class="second-step" v-if="secondstep">
            <transition name="left" appear>
            <div class="wrapper-end-buttons">
                <h2>YOU PICKED</h2>
                <button disabled class="result-btn" :class="yourChoice">
                    <div class="wrapper-img">
                        <img
                            v-if="yourChoice === 'paper'"
                            src="../assets/icon-paper.svg"
                            alt="icon"
                        />
                        <img
                            v-if="yourChoice === 'scissors'"
                            src="../assets/icon-scissors.svg"
                            alt="icon"
                        />
                        <img
                            v-if="yourChoice === 'rock'"
                            src="../assets/icon-rock.svg"
                            alt="icon"
                        />
                    </div>
                </button>
            </div>
            </transition>

            <transition name="fade" appear>
            <div class="result-text">
                <h2>
                    You <span>{{ result }}</span>
                </h2>
                <button @click="resetGame()" class="reset-btn">
                    Play again
                </button>
            </div>
            </transition>

            <transition name="right" appear>
            <div class="wrapper-end-buttons">
                <h2>THE HOUSE PICKED</h2>
                <button disabled class="result-btn" :class="computerChoice">
                    <div class="wrapper-img">
                        <img
                            v-if="computerChoice === 'paper'"
                            src="../assets/icon-paper.svg"
                            alt="icon"
                        />
                        <img
                            v-if="computerChoice === 'scissors'"
                            src="../assets/icon-scissors.svg"
                            alt="icon"
                        />
                        <img
                            v-if="computerChoice === 'rock'"
                            src="../assets/icon-rock.svg"
                            alt="icon"
                        />
                    </div>
                </button>
            </div>
            </transition>
        </section>
    </div>
</template>

<script>
import { ref } from 'vue';
export default {
    name: 'Game',
    setup() {
        const firststep = ref(true);
        const secondstep = ref(false);
        const options = ref(['rock', 'paper', 'scissors']);
        const yourChoice = ref('');
        const computerChoice = ref('');
        const result = ref('');
        const score = ref(0);

        const humanChoice = (choice) => {
            yourChoice.value = choice;
            computerAction();
            selectWinner();
            firststep.value = false;
            secondstep.value = true;
        };

        const computerAction = () => {
            computerChoice.value =
                options.value[Math.floor(Math.random() * options.value.length)];
        };

        const selectWinner = () => {
            if (yourChoice.value === computerChoice.value) {
                result.value = 'draw';
            } else if (
                (yourChoice.value === 'paper' &&
                    computerChoice.value === 'rock') ||
                (yourChoice.value === 'rock' &&
                    computerChoice.value === 'scissors') ||
                (yourChoice.value === 'scissors' &&
                    computerChoice.value === 'paper')
            ) {
                result.value = 'win';
                setTimeout(() => {
                  score.value++;
                }, 1000)
                
            } else {
                result.value = 'loose';
            }
        };

        const resetGame = () => {
            yourChoice.value = '';
            computerChoice.value = '';
            result.value = '';
            firststep.value = true;
            secondstep.value = false;
        };

        return {
            firststep,
            secondstep,
            options,
            yourChoice,
            humanChoice,
            computerChoice,
            computerAction,
            resetGame,
            result,
            score,
        };
    },
};
</script>

<style scoped lang="scss">
* {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
    font-family: 'Barlow Semi Condensed', sans-serif;
}

.container {
    min-height: 100vh;
    width: 900px;
    margin: 0 auto;
}

header {
    border: 3px solid rgba($color: #fff, $alpha: 0.5);
    border-radius: 10px;
    width: 100%;
    padding: 30px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 50px;
    margin-bottom: 50px;
    .score-wrapper {
        background: white;
        width: 150px;
        border-radius: 10px;
        display: flex;
        justify-content: center;
        flex-direction: column;
        align-items: center;
        padding: 15px;
        h2 {
            color: hsl(229, 64%, 46%);
            text-transform: uppercase;
            letter-spacing: 1px;
            font-size: 1.3rem;
            font-weight: 600;
        }
        span {
            color: hsl(229, 25%, 31%);
            font-size: 3rem;
            letter-spacing: 1px;
        }
    }
}
main {
    width: 650px;
    height: 450px;
    background: url('../assets/bg-triangle.svg') no-repeat 50% 65%;
    background-size: 55%;
    margin: 0 auto;
    position: relative;
}
.circle-btn {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 200px;
    height: 200px;
    border-radius: 50%;
    position: absolute;
    border: none;
    cursor: pointer;
    outline: none;
    transition: transform 0.1s ease;

    &:active {
        transform: scale(0.9);
        transition: transform 0.1s ease;
    }

    &.btn-paper {
        background: linear-gradient(hsl(230, 89%, 62%), hsl(230, 89%, 65%));
        left: 0;
        top: 0;
    }
    &.btn-scissors {
        background: linear-gradient(hsl(39, 89%, 49%), hsl(40, 84%, 53%));
        right: 0;
        top: 0;
    }
    &.btn-rock {
        background: linear-gradient(hsl(349, 71%, 52%), hsl(349, 70%, 56%));
        left: 50%;
        bottom: 0;
        transform: translateX(-50%);
        //overwrite due to the translateX(-50%)
        &:active {
            transform: translateX(-50%) scale(0.9);
            transition: transform 0.1s ease;
        }
    }

    .wrapper-img {
        background: white;
        width: 150px;
        height: 150px;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;

        img {
            width: 45%;
            height: 45%;
            display: block;
            object-fit: contain;
        }
    }
}

.second-step {
    width: 100%;
    height: 450px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    .result-text {
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        h2 {
            text-transform: uppercase;
            font-size: 2.8rem;
            letter-spacing: 1px;
        }
        .reset-btn {
            padding: 10px 30px;
            background: white;
            border-radius: 5px;
            font-size: 1.8em;
            font-weight: 600;
            text-transform: uppercase;
            border: none;
            cursor: pointer;
            margin-top: 20px;
            outline: none;
        }
    }
    .result-btn {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 200px;
        height: 200px;
        border-radius: 50%;
        border: none;
        cursor: pointer;
        outline: none;

        .wrapper-img {
            background: white;
            width: 150px;
            height: 150px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;

            img {
                width: 45%;
                height: 45%;
                display: block;
                object-fit: contain;
            }
        }

        &.rock {
            background: linear-gradient(hsl(349, 71%, 52%), hsl(349, 70%, 56%));
        }
        &.paper {
            background: linear-gradient(hsl(230, 89%, 62%), hsl(230, 89%, 65%));
        }
        &.scissors {
            background: linear-gradient(hsl(39, 89%, 49%), hsl(40, 84%, 53%));
        }
    }
}
.wrapper-end-buttons {
    text-align: center;
    h2 {
        padding-bottom: 60px;
        font-weight: 600;
    }
}

@media screen and (max-width: 930px) {
    .container {
        width: 350px;
    }
    header {
        margin-bottom: 70px;
        .score-wrapper {
            width: 80px;
        }
    }
    main {
        width: 350px;
        height: 300px;
    }
    .circle-btn {
        width: 130px;
        height: 130px;
        .wrapper-img {
            width: 90px;
            height: 90px;
        }
    }

    .second-step {
        height: auto;
        .result-text {
            order: 1;
            margin-top: 30px;
            width: 100%;
        }
        .result-btn {
            width: 130px;
            height: 130px;
            .wrapper-img {
                width: 90px;
                height: 90px;
            }
        }
    }

    .wrapper-end-buttons {
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        text-align: center;
        h2 {
            padding-bottom: 30px;
            margin-top: 15px;
            order: 1;
            font-size: 1.1rem;
        }
    }
}


//TRANSITION VUE JS

.opacity-main-enter-from{
  opacity: 0;
}

.opacity-main-enter-active{
  transition:all .7s ease;
}

.fade-enter-from{
  opacity: 0;
  transform: scale(0);
}

.fade-enter-active{
  transition:all .6s ease 1s;
}


.left-enter-from{
  opacity: 0;
  transform: translateX(-100%);
}

.left-enter-active{
  transition:all 1s ease;
}

.right-enter-from{
  opacity: 0;
  transform: translateX(100%);
}

.right-enter-active{
  transition:all 1s ease;
}


</style>
