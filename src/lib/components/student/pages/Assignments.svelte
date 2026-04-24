<!-- Assignments.svelte -->
<script lang="ts">
	import { getContext } from 'svelte';

	const i18n = getContext('i18n');

	let selectedSubject = 'All Subjects';
	let selectedStatus = 'All Status';

	// Mock data for the design
	const assignments = [
		{
			id: 1,
			subject: 'Mathematics',
			points: 10,
			title: 'Calculus Integration Problems',
			description:
				'Complete the set of 5 calculus integration problems. Show all steps of your work and explain your approach for each problem.',
			date: 'Due: April 20,2025',
			status: 'in-progress',
			buttonText: 'Continue',
			buttonStyle: 'bg-blue-500 hover:bg-blue-600 text-white',
			topColor: 'bg-blue-500'
		},
		{
			id: 2,
			subject: 'Physics',
			points: 10,
			title: 'Kinematics Equations',
			description:
				'Solve the 10 kinematics problems provided in the worksheet. Ensure you show all formulas used.',
			date: 'Due: April 22,2025',
			status: 'todo',
			buttonText: 'Start',
			buttonStyle: 'bg-blue-500 hover:bg-blue-600 text-white',
			topColor: 'bg-gray-400'
		},
		{
			id: 3,
			subject: 'Mathematics',
			points: 10,
			title: 'Trigonometry Quiz',
			description:
				'Review the feedback from your recent trigonometry quiz to understand your mistakes.',
			date: 'Completed: April 10,2025',
			status: 'completed',
			buttonText: 'View Feedback',
			buttonStyle: 'bg-white hover:bg-gray-50 text-gray-700 border border-gray-300 dark:bg-gray-800 dark:hover:bg-gray-700 dark:text-gray-300 dark:border-gray-600',
			topColor: 'bg-green-500'
		},
		{
			id: 4,
			subject: 'Chemistry',
			points: 10,
			title: 'Balancing Chemical Equations',
			description:
				'Complete the balancing equations assignment. You are late, so please submit it as soon as possible.',
			date: 'Due: April 5,2025 (Late)',
			status: 'late',
			buttonText: 'Submit Late',
			buttonStyle: 'bg-blue-500 hover:bg-blue-600 text-white',
			topColor: 'bg-red-500'
		},
		{
			id: 5,
			subject: 'Physics',
			points: 10,
			title: 'Newton\'s Laws Lab Report',
			description:
				'Write down the methodology and results for the Newton\'s laws experiment we did in class.',
			date: 'Due: April 25,2025',
			status: 'in-progress',
			buttonText: 'Continue',
			buttonStyle: 'bg-blue-500 hover:bg-blue-600 text-white',
			topColor: 'bg-blue-500'
		}
	];

	// Reactive filtering logic
	$: filteredAssignments = assignments.filter((item) => {
		const matchSubject = selectedSubject === 'All Subjects' || item.subject === selectedSubject;
		
		let matchStatus = true;
		if (selectedStatus === 'In Progress') matchStatus = item.status === 'in-progress';
		else if (selectedStatus === 'To Do') matchStatus = item.status === 'todo';
		else if (selectedStatus === 'Completed') matchStatus = item.status === 'completed';
		else if (selectedStatus === 'Late') matchStatus = item.status === 'late';
		
		return matchSubject && matchStatus;
	});
</script>

<div class="bg-white dark:bg-gray-800 rounded-[32px] p-8 md:p-10 w-full flex flex-col shadow-sm border border-gray-100 dark:border-gray-700 min-h-[calc(100vh-140px)] transition-colors duration-200">
	<!-- Header -->
	<div class="mb-10">
		<h1 class="text-2xl md:text-[28px] font-semibold text-[#2b3674] dark:text-gray-100 mb-2">Welcome back, Karim!</h1>
		<p class="text-[#a3aed1] dark:text-gray-400 text-base font-medium">
			You have 3 upcoming deadlines this week. Stay focused and keep learning !
		</p>
	</div>

	<!-- Toolbar -->
	<div class="flex flex-col md:flex-row md:items-center justify-between mb-8 gap-4">
		<h2 class="text-xl font-bold text-[#2b3674] dark:text-gray-100">My Assignments</h2>

		<div class="flex items-center space-x-3 text-sm">
			<!-- All Subjects Dropdown -->
			<select
				bind:value={selectedSubject}
				class="bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 text-gray-500 dark:text-gray-300 py-2 pl-5 pr-8 rounded-full focus:outline-none focus:border-blue-500 cursor-pointer font-medium transition-colors"
			>
				<option value="All Subjects">All Subjects</option>
				<option value="Mathematics">Mathematics</option>
				<option value="Physics">Physics</option>
				<option value="Chemistry">Chemistry</option>
			</select>

			<!-- All Status Dropdown -->
			<select
				bind:value={selectedStatus}
				class="bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 text-gray-500 dark:text-gray-300 py-2 pl-5 pr-8 rounded-full focus:outline-none focus:border-blue-500 cursor-pointer font-medium transition-colors"
			>
				<option value="All Status">All Status</option>
				<option value="To Do">To Do</option>
				<option value="In Progress">In Progress</option>
				<option value="Completed">Completed</option>
				<option value="Late">Late</option>
			</select>

			<!-- Filter Button (Optional reset or extra logic) -->
			<button
				on:click={() => { selectedSubject = 'All Subjects'; selectedStatus = 'All Status'; }}
				class="bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 text-gray-500 dark:text-gray-300 py-2 px-6 rounded-full hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors font-medium"
				title="Reset Filters"
			>
				Reset
			</button>
		</div>
	</div>

	<!-- Grid -->
	<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 mb-8 flex-grow">
		{#if filteredAssignments.length === 0}
			<div class="col-span-full flex items-center justify-center py-12 text-gray-400 dark:text-gray-500 font-medium">
				No assignments found matching these filters.
			</div>
		{/if}
		{#each filteredAssignments as item (item.id)}
			<div
				class="bg-white dark:bg-gray-800 rounded-2xl border border-gray-200 dark:border-gray-700 shadow-sm overflow-hidden flex flex-col relative transition-colors duration-200"
			>
				<!-- Colored Top Border -->
				<div class="absolute top-0 left-0 right-0 h-[6px] {item.topColor}"></div>

				<div class="p-6 pt-7 flex flex-col flex-grow">
					<!-- Badges -->
					<div class="flex justify-between items-center mb-5">
						<span class="bg-[#e8f0fe] dark:bg-blue-900/30 text-blue-600 dark:text-blue-400 text-xs font-semibold px-3 py-1.5 rounded-[8px]">
							{item.subject}
						</span>
						<span class="text-[#a3aed1] dark:text-gray-400 text-xs font-semibold">
							{item.points} points
						</span>
					</div>

					<!-- Content -->
					<h3 class="text-[#2b3674] dark:text-gray-100 font-bold text-[15px] mb-2">{item.title}</h3>
					<p class="text-[#a3aed1] dark:text-gray-400 text-[13px] leading-relaxed mb-6 flex-grow font-medium">
						{item.description}
					</p>

					<!-- Footer -->
					<div class="pt-5 border-t border-gray-100 dark:border-gray-700 flex justify-between items-center mt-auto">
						<span class="text-[#a3aed1] dark:text-gray-400 text-xs font-medium">
							{item.date}
						</span>
						<button
							class="{item.buttonStyle} text-xs font-semibold px-4 py-2 rounded-lg transition-colors shadow-sm"
						>
							{item.buttonText}
						</button>
					</div>
				</div>
			</div>
		{/each}
	</div>

	<!-- Pagination -->
	<div class="flex justify-between items-center mt-auto pt-6 px-4">
		<button
			class="text-gray-300 dark:text-gray-600 font-bold text-sm hover:text-gray-400 dark:hover:text-gray-500 transition-colors cursor-not-allowed"
			disabled
		>
			Back
		</button>
		<button
			class="text-gray-300 dark:text-gray-600 font-bold text-sm hover:text-gray-400 dark:hover:text-gray-500 transition-colors cursor-not-allowed"
			disabled
		>
			Next
		</button>
	</div>
</div>
