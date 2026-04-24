<!-- Classrooms.svelte -->
<script lang="ts">
	import { getContext } from 'svelte';

	const i18n = getContext('i18n');

	// Mock data using local CSS gradients and initials instead of external images
	const courses = [
		{
			id: 1,
			title: 'Introduction to Algorithmic',
			teacher: 'Samia AHMED',
			subject: 'Computer Science',
			teacherInitial: 'S',
			teacherColor: 'bg-purple-500',
			imageClass: 'bg-gradient-to-br from-indigo-400 via-purple-400 to-pink-400',
			iconPath: 'M9 3v2m6-2v2M9 19v2m6-2v2M5 9H3m2 6H3m18-6h-2m2 6h-2M7 19h10a2 2 0 002-2V7a2 2 0 00-2-2H7a2 2 0 00-2 2v10a2 2 0 002 2zM9 9h6v6H9V9z',
			level: 'Beginner',
			levelColor: 'text-purple-500',
		},
		{
			id: 2,
			title: 'The rise and explanation of Islam',
			teacher: 'Ibrahim AMINE',
			subject: 'History',
			teacherInitial: 'I',
			teacherColor: 'bg-blue-500',
			imageClass: 'bg-gradient-to-br from-blue-400 via-cyan-400 to-teal-400',
			iconPath: 'M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253',
			level: 'Advanced',
			levelColor: 'text-blue-500',
		},
		{
			id: 3,
			title: 'Number Sense and Calculations',
			teacher: 'Ahmed SAMI',
			subject: 'Mathematics',
			teacherInitial: 'A',
			teacherColor: 'bg-green-500',
			imageClass: 'bg-gradient-to-br from-emerald-400 via-green-400 to-lime-400',
			iconPath: 'M9 7h6m0 10v-3m-3 3h.01M9 17h.01M9 14h.01M12 14h.01M15 11h.01M12 11h.01M9 11h.01M7 21h10a2 2 0 002-2V5a2 2 0 00-2-2H7a2 2 0 00-2 2v14a2 2 0 002 2z',
			level: 'Intermediate',
			levelColor: 'text-green-500',
		},
		{
			id: 4,
			title: 'Introduction to Big Data',
			teacher: 'Samia AHMED',
			subject: 'Computer Science',
			teacherInitial: 'S',
			teacherColor: 'bg-purple-500',
			imageClass: 'bg-gradient-to-br from-orange-400 via-red-400 to-rose-400',
			iconPath: 'M4 7v10c0 2.21 3.582 4 8 4s8-1.79 8-4V7M4 7c0 2.21 3.582 4 8 4s8-1.79 8-4M4 7c0-2.21 3.582-4 8-4s8 1.79 8 4m0 5c0 2.21-3.582 4-8 4s-8-1.79-8-4',
			level: 'Intermediate',
			levelColor: 'text-green-500',
		}
	];

	let selectedSubject = 'All Subjects';
	let searchQuery = '';
	
	// Reactive filtering logic
	$: filteredCourses = courses.filter(course => {
		const matchSubject = selectedSubject === 'All Subjects' || course.subject === selectedSubject;
		const matchSearch = course.title.toLowerCase().includes(searchQuery.toLowerCase()) || 
							course.teacher.toLowerCase().includes(searchQuery.toLowerCase());
		return matchSubject && matchSearch;
	});
</script>

<div class="flex flex-col h-full">
	<!-- Top Greeting Header -->
	<div class="mb-6 px-2">
		<h1 class="text-2xl md:text-[28px] font-bold text-[#2b3674] dark:text-gray-100 mb-1 tracking-tight">
			Hello Karim
		</h1>
		<p class="text-[#a3aed1] dark:text-gray-400 text-sm font-medium">Let's learn something new today!</p>
	</div>

	<!-- White Main Container -->
	<div class="bg-white dark:bg-gray-800 rounded-[32px] p-8 md:p-10 w-full flex flex-col shadow-sm border border-gray-100 dark:border-gray-700 min-h-[calc(100vh-200px)] transition-colors duration-200">
		
		<!-- Toolbar -->
		<div class="flex flex-col lg:flex-row lg:items-center justify-between mb-10 gap-4">
			<h2 class="text-xl font-bold text-[#2b3674] dark:text-gray-100">My Courses</h2>

			<div class="flex flex-col sm:flex-row items-center space-y-3 sm:space-y-0 sm:space-x-3 text-sm w-full lg:w-auto">
				
				<!-- Search Bar -->
				<div class="relative w-full sm:w-auto">
					<svg class="absolute left-3 top-3 w-4 h-4 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path></svg>
					<input 
						type="text" 
						bind:value={searchQuery} 
						placeholder="Rechercher un cours ou professeur..." 
						class="w-full sm:w-64 bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 text-gray-700 dark:text-gray-200 py-2.5 pl-9 pr-4 rounded-full focus:outline-none focus:border-blue-500 focus:ring-1 focus:ring-blue-500 transition-colors shadow-sm" 
					/>
				</div>

				<!-- All Subjects Dropdown -->
				<select
					bind:value={selectedSubject}
					class="w-full sm:w-auto bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 text-gray-500 dark:text-gray-300 py-2.5 pl-5 pr-8 rounded-full focus:outline-none focus:border-blue-500 focus:ring-1 focus:ring-blue-500 cursor-pointer font-medium transition-colors shadow-sm"
				>
					<option value="All Subjects">All Subjects</option>
					<option value="Mathematics">Mathematics</option>
					<option value="Computer Science">Computer Science</option>
					<option value="History">History</option>
				</select>
			</div>
		</div>

		<!-- Grid of Courses -->
		<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-3 2xl:grid-cols-4 gap-6 mb-8 flex-grow">
			{#each filteredCourses as course (course.id)}
				<div class="bg-white dark:bg-gray-800 rounded-3xl border border-gray-100 dark:border-gray-700 shadow-[0_4px_15px_-4px_rgba(6,81,237,0.08)] dark:shadow-none overflow-visible flex flex-col transition-transform hover:-translate-y-1 duration-300">
					
					<!-- Image Header -->
					<div class="relative h-36 w-full rounded-t-3xl overflow-visible {course.imageClass} flex items-center justify-center">
						<!-- Custom Icon overlay -->
						<svg class="w-16 h-16 text-white/40 drop-shadow-sm" fill="none" stroke="currentColor" viewBox="0 0 24 24">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d={course.iconPath}></path>
						</svg>
						
						<!-- Teacher Pill Overlapping -->
						<div class="absolute -bottom-4 left-5 bg-white dark:bg-gray-800 rounded-full shadow-md px-2.5 py-1.5 flex items-center space-x-2 border border-gray-50 dark:border-gray-700 z-10 transition-colors">
							<div class="w-6 h-6 rounded-full {course.teacherColor} flex items-center justify-center text-white text-[11px] font-bold shadow-sm">
								{course.teacherInitial}
							</div>
							<span class="text-xs font-bold text-gray-800 dark:text-gray-200">{course.teacher}</span>
						</div>
					</div>

					<!-- Content -->
					<div class="p-6 pt-8 flex flex-col flex-grow">
						<h3 class="text-[#2b3674] dark:text-gray-100 font-bold text-[15px] mb-6 flex-grow leading-snug pr-2">{course.title}</h3>
						
						<!-- Footer of card -->
						<div class="flex justify-between items-center mt-auto">
							<span class="text-xs font-bold {course.levelColor}">{course.level}</span>
							<button class="bg-blue-500 hover:bg-blue-600 text-white text-xs font-bold px-6 py-2.5 rounded-[10px] transition-colors shadow-sm shadow-blue-500/30">
								Start
							</button>
						</div>
					</div>
				</div>
			{:else}
				<div class="col-span-full flex flex-col items-center justify-center py-12 text-gray-400 dark:text-gray-500">
					<svg class="w-12 h-12 mb-3 opacity-50" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.172 16.172a4 4 0 015.656 0M9 10h.01M15 10h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
					<p class="font-medium text-center">Aucun cours ne correspond à votre recherche.</p>
				</div>
			{/each}
		</div>

		<!-- Pagination -->
		<div class="flex justify-between items-center mt-auto pt-6 px-4">
			<button class="text-gray-300 dark:text-gray-600 font-bold text-sm hover:text-gray-400 dark:hover:text-gray-500 transition-colors cursor-not-allowed" disabled>
				Back
			</button>
			<button class="text-gray-300 dark:text-gray-600 font-bold text-sm hover:text-gray-400 dark:hover:text-gray-500 transition-colors cursor-not-allowed" disabled>
				Next
			</button>
		</div>
	</div>
</div>
