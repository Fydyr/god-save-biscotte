<template>
    <!-- Boutons qui apparaissent progressivement à des positions aléatoires -->
    <transition name="slide-up">
        <img
            v-if="currentStep >= 0"
            :src="getButtonImage(0)"
            @click="toggleButton(0)"
            class="button random-position"
            :style="buttonPositions[0]"
            alt="button 1"
        />
    </transition>

    <transition name="slide-up">
        <img
            v-if="currentStep >= 1"
            :src="getButtonImage(1)"
            @click="toggleButton(1)"
            class="button random-position"
            :style="buttonPositions[1]"
            alt="button 2"
        />
    </transition>

    <transition name="slide-up">
        <img
            v-if="currentStep >= 2"
            :src="getButtonImage(2)"
            @click="toggleButton(2)"
            class="button random-position"
            :style="buttonPositions[2]"
            alt="button 3"
        />
    </transition>

    <transition name="slide-up">
        <img
            v-if="currentStep >= 3"
            :src="getButtonImage(3)"
            @click="toggleButton(3)"
            class="button random-position"
            :style="buttonPositions[3]"
            alt="button 4"
        />
    </transition>

    <!-- Parchemin avec transition -->
    <transition name="fade">
        <div v-if="showParchment" class="parchment-overlay" @click="closeParchment">
            <img src="../assets/img/scroll.png" class="parchment" alt="Parchemin" />
        </div>
    </transition>
</template>

<style>
.parchment-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 1000;
    cursor: pointer;
}

.parchment {
    width: 100vw;
    height: 100vh;
    object-fit: contain;
    cursor: default;
}

/* Animation fade pour le parchemin */
.fade-enter-active,
.fade-leave-active {
    transition: opacity 1s;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}

/* Animation slide-up pour les boutons */
.slide-up-enter-active {
    transition: all 0.6s ease-out;
}

.slide-up-enter-from {
    opacity: 0;
    transform: translateY(50px) scale(0.8);
}

.button {
    cursor: pointer;
    transition: transform 0.1s;
    height: 200px;
    width: 200px;
}

.button:active {
    transform: scale(0.95) !important;
}

.random-position {
    position: fixed;
    z-index: 100;
}
</style>

<script>
export default {
    data() {
        return {
            currentStep: 0, // Étape actuelle (0 = premier bouton visible)
            clickedButtons: [false, false, false, false], // État de chaque bouton
            showParchment: false,
            buttonPositions: [] // Positions aléatoires des boutons
        }
    },
    mounted() {
        // Générer des positions aléatoires pour chaque bouton
        this.generateRandomPositions();

        // Regénérer les positions si la fenêtre est redimensionnée
        window.addEventListener('resize', this.generateRandomPositions);
    },
    beforeUnmount() {
        window.removeEventListener('resize', this.generateRandomPositions);
    },
    methods: {
        generateRandomPositions() {
            const positions = [];
            const buttonSize = 200; // Taille du bouton (200px)
            const padding = 50; // Marge minimum par rapport aux bords

            // Zone du chatbot (en bas à droite)
            const chatbotZone = {
                top: window.innerHeight - 400, // Zone du chatbot + marge (image + bulle + form)
                left: window.innerWidth - 400, // Zone du chatbot + marge
                width: 400,
                height: 400
            };

            for (let i = 0; i < 4; i++) {
                let position;
                let attempts = 0;
                let validPosition = false;

                // Essayer de trouver une position qui ne chevauche pas les autres boutons
                while (!validPosition && attempts < 100) {
                    const top = Math.random() * (window.innerHeight - buttonSize - padding * 2) + padding;
                    const left = Math.random() * (window.innerWidth - buttonSize - padding * 2) + padding;

                    position = {
                        top: `${top}px`,
                        left: `${left}px`
                    };

                    // Vérifier si cette position ne chevauche PAS la zone du chatbot
                    const overlapsChatbot = (
                        top < chatbotZone.top + chatbotZone.height &&
                        top + buttonSize > chatbotZone.top &&
                        left < chatbotZone.left + chatbotZone.width &&
                        left + buttonSize > chatbotZone.left
                    );

                    if (overlapsChatbot) {
                        validPosition = false;
                        attempts++;
                        continue;
                    }

                    // Vérifier si cette position chevauche les positions existantes
                    validPosition = true;
                    for (let j = 0; j < positions.length; j++) {
                        const existingTop = parseFloat(positions[j].top);
                        const existingLeft = parseFloat(positions[j].left);

                        const distance = Math.sqrt(
                            Math.pow(top - existingTop, 2) +
                            Math.pow(left - existingLeft, 2)
                        );

                        // Si la distance est trop petite, essayer une nouvelle position
                        if (distance < buttonSize + 50) {
                            validPosition = false;
                            break;
                        }
                    }

                    attempts++;
                }

                positions.push(position);
            }

            this.buttonPositions = positions;
        },
        getButtonImage(index) {
            // Retourne l'image du bouton (pressé ou normal)
            return this.clickedButtons[index]
                ? new URL('../assets/img/pressed_button.png', import.meta.url).href
                : new URL('../assets/img/button.png', import.meta.url).href;
        },
        toggleButton(index) {
            // Ne faire quelque chose que si c'est le dernier bouton apparu
            if (index !== this.currentStep) return;

            // Marquer le bouton comme cliqué
            this.clickedButtons[index] = true;

            // Retour automatique après 200ms
            setTimeout(() => {
                this.clickedButtons[index] = false;
                // Afficher le parchemin après l'animation du bouton
                this.showParchment = true;
            }, 200);
        },
        closeParchment() {
            this.showParchment = false;

            // Passer à l'étape suivante après fermeture du parchemin
            if (this.currentStep < 3) {
                setTimeout(() => {
                    this.currentStep++;
                }, 300); // Petit délai pour une transition plus fluide
            }
        }
    }
}
</script>