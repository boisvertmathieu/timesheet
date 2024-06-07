<script lang="ts">
	export let selectedWeek: {
		weekNumber: number;
		startDate: string;
		endDate: string;
		days: {
			date: string;
			dayName: string;
		}[];
	};

	const workSchedule = {
		'2024-06-02': [
			{ start: '09:00', end: '12:00', type: 'work' },
			{ start: '13:00', end: '17:00', type: 'work' }
		],
		'2024-06-03': [
			{ start: '09:00', end: '12:00', type: 'work' },
			{ start: '13:00', end: '17:00', type: 'work' }
		]
		// Ajoutez d'autres jours ici
	};
</script>

<div class="grid grid-cols-7 gap-4 mt-6">
	{#each selectedWeek.days as day}
		<div class="border rounded-lg p-4">
			<div class="font-bold text-center mb-2">
				<span>{day.dayName}</span>
				<span class="block text-sm text-gray-500">{day.date}</span>
			</div>
			<div class="flex flex-col gap-2">
				{#each workSchedule[day.date] || [] as period}
					<div
						class="p-2 rounded text-center text-white {period.type === 'work'
							? 'bg-blue-500'
							: 'bg-orange-500'}"
					>
						{period.start} - {period.end}
					</div>
				{/each}
			</div>
		</div>
	{/each}
</div>
