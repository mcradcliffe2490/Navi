<script lang="ts">
	import { goto } from '$app/navigation';
	import { onMount } from 'svelte';
	import FormField from '@smui/form-field';
	import Radio from '@smui/radio';
	import Button from '@smui/button';
	import Checkbox from '@smui/checkbox';

	interface Answers {
		[question: string]: any;
	}

	interface Questions {
		[question: string]: any;
	}

	interface Selected {
		[question: string]: any;
	}

	onMount(() => {
		import('@smui/checkbox/bare.css');
		import('@smui/form-field/bare.css');
		import('@smui/button/bare.css');
	});

	const questions: Questions = {
		question1: {
			question: "What's your main reason for building the computer?",
			options: ['Gaming', 'Photo/Video editing', 'Casual Use'],
			maxSelections: 1,
			displayWarning: false
		},
		question2: {
			question:
				'Which of these games do you plan to spend the most time on or are most similar to games you play (choose three)?',
			options: [
				'Counter-Strike: Global Offensive',
				'Destiny 2',
				'Apex Legends',
				'Warzone',
				'Elden Ring',
				'Minecraft',
				'Fortnite',
				'League of Legends',
				'Valorant',
				'Rocket League',
				'Overwatch',
				'Grand Theft Auto V',
				'Indie Games',
				'Halo',
				'Forza Horizon 5',
				'God of War'
			],
			maxSelections: 3,
			displayWarning: false
		},
		question3: {
			question: 'What settings do you want to play on most of the time?',
			options: ['Low', 'Medium', 'High'],
			maxSelections: 1,
			displayWarning: false
		},
		question4: {
			question:
				'What resolution is your monitor/what resolution are you targeting most of the time?',
			options: ['1080p', '1440p', '2160p(4k)'],
			maxSelections: 1,
			displayWarning: false
		},
		question5: {
			question: 'What fps (frames per second) do you want to target most of the time?',
			options: ['60fps', '100fps', '120fps', '144fps'],
			maxSelections: 1,
			displayWarning: false
		},
		question6: {
			question: "What's your budget?",
			options: ['~$1000', '$1000-$1500', '$1500-$2000', '$2000+'],
			maxSelections: 1,
			displayWarning: false
		}
	};

	let answers: Answers = {
		question1: [],
		question2: [],
		question3: [],
		question4: [],
		question5: [],
		question6: []
	};

	let selected: Selected = {
		question1: [],
		question2: [],
		question3: [],
		question4: [],
		question5: [],
		question6: []
	};

	function toggleChecked(value: string, key: string) {
		const selectedOptions = selected[key];
		const isChecked = selectedOptions.includes(value);
		const validOptions = answers[key];
		const isSelected = validOptions.includes(value);
		if (isChecked) {
			// removing the current option from checked list
			selected[key] = selectedOptions.filter((option: string) => option !== value);
			if (selected[key].length <= questions[key].maxSelections) {
				questions[key].displayWarning = false;
				answers[key] = selected[key];
			}
		} else {
			selected[key] = [...selectedOptions, value];
			if (selected[key].length > questions[key].maxSelections) {
				questions[key].displayWarning = true;
			} else {
				answers[key] = selected[key];
			}
		}
	}

	function submitForm() {




		goto('/result')
	}
	// set visibility of questions based on if other questions have been answered
</script>

<main>
	<form>
		<div>
			{#each Object.entries(questions) as [key, value]}
				<FormField align="end" class="smui-checkbox">
					<h2 class="nowrap">{value.question}</h2>
					{#each value.options as option}
						<div>
							<Checkbox
								on:change={() => toggleChecked(option, key)}
								value={option}
								id={`checkbox-${key}-${option}`}
							/> <label for={`checkbox-${key}-${option}`}>{option}</label>
						</div>
					{/each}
				</FormField>
				<div>
					{#if value.displayWarning}
						<h3 style="color: red;">You have selected too many options</h3>
					{/if}
				</div>
			{/each}
		</div>
		<Button style="color: #000080;" on:click={() => submitForm()}>Submit</Button>
	</form>
	<Button style="color: #000080;" on:click={() => goto('/')}>Home</Button>
</main>

<style>
	
	.nowrap {
		color: #007FFF;
		white-space: nowrap;
	}
	:global(.smui-checkbox) {
		display: block !important;
		margin-left: 20px;
	}
</style>
