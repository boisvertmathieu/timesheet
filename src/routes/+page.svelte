<script lang="ts">
	import { endOfWeek, eachWeekOfInterval, eachDayOfInterval, format } from 'date-fns';
	import { fr } from 'date-fns/locale';
	import WeekCalendar from '../components/WeekCalendar.svelte';

	const currentYear: number = new Date().getFullYear();

	const startOfYear = new Date(currentYear, 0, 1);
	const endOfYear = new Date(currentYear, 11, 31);

	const capitalizeFirstLetter = (string: string) => {
		return string.charAt(0).toUpperCase() + string.slice(1);
	};

	// Building weeks map
	const weeks = eachWeekOfInterval({ start: startOfYear, end: endOfYear }, { weekStartsOn: 0 }).map(
		(weekStart, index) => {
			const weekEnd = endOfWeek(weekStart, { weekStartsOn: 0 });
			return {
				weekNumber: index + 1,
				startDate: format(weekStart, 'yyyy-MM-dd'),
				endDate: format(weekEnd, 'yyyy-MM-dd'),
				days: eachDayOfInterval({ start: weekStart, end: weekEnd }).map((day) => ({
					date: format(day, 'yyyy-MM-dd'),
					dayName: capitalizeFirstLetter(format(day, 'EEEE', { locale: fr }))
				}))
			};
		}
	);

	let selectedWeek = weeks[0];

	const handleWeekChange = (event: Event) => {
		let weekNum: number = parseInt((event.target as HTMLSelectElement).value);
		console.log('Week num changed to : ', weekNum);
		selectedWeek = weeks[weekNum];
	};
</script>

<main class="p-6">
	<label for="weekSelect" class="block text-gray-700 mb-2">Choisissez une semaine :</label>
	<select id="weekSelect" class="border rounded p-2 mb-4" on:change={handleWeekChange}>
		{#each weeks as week, index}
			<option value={index}>
				Semaine {week.weekNumber} (du {week.startDate} au {week.endDate})
			</option>
		{/each}
	</select>

	<p class="mb-2">Semaine sélectionnée : {selectedWeek.weekNumber}</p>
	<p class="mb-4">Dates : {selectedWeek.startDate} - {selectedWeek.endDate}</p>

	<WeekCalendar {selectedWeek} />
</main>
