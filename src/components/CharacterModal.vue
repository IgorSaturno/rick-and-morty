<script setup>
import { computed } from "vue";

const props = defineProps({
  character: {
    type: Object,
    required: false,
    default: null,
  },
  isOpen: {
    type: Boolean,
    default: false,
  },
});

const emit = defineEmits(["close"]);

const closeModal = () => {
  emit("close");
};

const hasCharacter = computed(() => props.character !== null);

const stopPropagation = (event) => {
  event.stopPropagation();
};
</script>

<template>
  <Teleport to="body">
    <Transition name="modal">
      <div
        v-if="isOpen && hasCharacter"
        class="modal-overlay"
        @click="closeModal"
      >
        <div class="modal-content" @click="stopPropagation">
          <button class="close-button" @click="closeModal">X</button>

          <div class="modal-body">
            <div class="modal-image">
              <img :src="character.image" :alt="character.name" />
            </div>

            <div class="modal-info">
              <h2>{{ character.name }}</h2>
              <div class="info-grid">
                <div class="info-item">
                  <span class="info-label">Status:</span>
                  <span
                    :class="[
                      'info-value',
                      `status-${character.status.toLowerCase()}`,
                    ]"
                    >{{ character.status }}</span
                  >
                </div>

                <div class="info-item">
                  <span class="info-label">Espécie:</span>
                  <span class="info-value">{{ character.species }}</span>
                </div>

                <div class="info-item" v-if="character.type">
                  <span class="info-label">Tipo:</span>
                  <span class="info-value">{{ character.type }}</span>
                </div>

                <div class="info-item">
                  <span class="info-label">Gênero:</span>
                  <span class="info-value">{{ character.gender }}</span>
                </div>

                <div class="info-item">
                  <span class="info-label">Origem:</span>
                  <span class="info-value">{{ character.origin.name }}</span>
                </div>

                <div class="info-item">
                  <span class="info-label">Localização:</span>
                  <span class="info-value">{{ character.location.name }}</span>
                </div>

                <div class="info-item">
                  <span class="info-label">Episódios:</span>
                  <span class="info-value"
                    >{{ character.episode.length }} episódios</span
                  >
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.75);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
  padding: 20px;
  backdrop-filter: blur(5px);
}

/* Conteúdo do modal */
.modal-content {
  background-color: var(--bg-card);
  border-radius: 16px;
  max-width: 800px;
  width: 100%;
  max-height: 90vh;
  overflow-y: auto;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
  position: relative;
}

/* Botão fechar */
.close-button {
  position: absolute;
  top: 15px;
  right: 15px;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background-color: var(--bg-secondary);
  color: var(--text-primary);
  border: 2px solid var(--border-color);
  font-size: 20px;
  z-index: 10;
  transition: all 0.3s ease;
  display: flex;
  justify-content: center;
  align-items: center;
}

.close-button:hover {
  background-color: #ff4444;
  color: white;
  border-color: #ff4444;
  transform: rotate(90deg);
}

/* Corpo do modal */
.modal-body {
  display: grid;
  grid-template-columns: 1fr 2fr;
  gap: 30px;
  padding: 30px;
}

/* Imagem */
.modal-image {
  position: relative;
}

.modal-image img {
  width: 100%;
  border-radius: 12px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
}

/* Informações */
.modal-info h2 {
  color: var(--text-primary);
  margin-bottom: 25px;
  font-size: 2em;
  border-bottom: 3px solid #646cff;
  padding-bottom: 10px;
}

.info-grid {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 15px;
}

.info-item {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

.info-label {
  color: var(--text-secondary);
  font-size: 0.85em;
  text-transform: uppercase;
  font-weight: 600;
  letter-spacing: 1px;
}

.info-value {
  color: var(--text-primary);
  font-size: 1.1em;
  font-weight: 500;
}

/* Status com cores */
.status-alive {
  color: #55cc44;
}

.status-dead {
  color: #ff4444;
}

.status-unknown {
  color: #999999;
}

/* Transições */
.modal-enter-active,
.modal-leave-active {
  transition: opacity 0.3s ease;
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;
}

.modal-enter-active .modal-content,
.modal-leave-active .modal-content {
  transition: transform 0.3s ease;
}

.modal-enter-from .modal-content,
.modal-leave-to .modal-content {
  transform: scale(0.9);
}

/* Responsivo */
@media (max-width: 768px) {
  .modal-body {
    grid-template-columns: 1fr;
    padding: 20px;
  }

  .modal-info h2 {
    font-size: 1.5em;
  }
}
</style>
