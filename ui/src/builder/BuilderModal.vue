<template>
	<Teleport to="#modal">
		<div class="BuilderModal" v-on:keydown="handleKeydown" tabindex="-1">
			<div class="main">
				<div class="titleContainer">
					<i v-if="icon" :class="`ri-${icon}-line`" class="ri-lg"></i>
					<h2>{{ title }}</h2>
					<button
						:title="closeAction?.desc ?? 'Close'"
						v-on:click="closeAction.fn"
					>
						<i class="ri-close-line ri-lg"></i>
					</button>
				</div>
				<div class="slotContainer">
					<slot></slot>
				</div>
				<div class="actionContainer" v-if="menuActions?.length > 0">
					<button
						v-for="action in menuActions"
						v-on:click="action.fn"
					>
						{{ action.desc }}
					</button>
				</div>
			</div>
		</div>
	</Teleport>
</template>

<script setup lang="ts">
import { toRefs } from "vue";

export type ModalAction = {
	desc: string;
	fn: Function;
};

const props = defineProps<{
	title: string;
	icon?: string;
	closeAction: ModalAction;
	menuActions?: ModalAction[];
}>();

const { title, icon, closeAction, menuActions } = toRefs(props);

const handleKeydown = (ev: KeyboardEvent) => {
	if (ev.key == "Escape") {
		closeAction.value.fn();
	}
};
</script>

<style scoped>
@import "./sharedStyles.css";

.BuilderModal {
	background: rgba(0, 0, 0, 0.1);
	width: 100vw;
	height: 100vh;
	display: flex;
	align-items: center;
	justify-content: center;
}

.main {
	background: white;
	width: 80%;
	max-width: 120ch;
	border-radius: 8px;
	box-shadow: 0 0 16px 4px rgba(0, 0, 0, 0.3);
}

.titleContainer {
	border-bottom: 1px solid var(--builderSubtleSeparatorColor);
	padding: 16px;
	display: flex;
	align-items: center;
}

.titleContainer > i {
	margin-right: 8px;
}

.titleContainer button {
	margin-left: auto;
}

.slotContainer {
	padding: 16px;
}

.actionContainer {
	border-top: 1px solid var(--builderSubtleSeparatorColor);
	padding: 16px;
}
</style>
