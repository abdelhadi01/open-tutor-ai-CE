<!-- Messages.svelte -->
<script lang="ts">
	import { getContext } from 'svelte';

	const i18n = getContext('i18n');

	let activeChannelId = 1;

	// Mock data updated to include all channels
	const channels = [
		{ id: 1, name: 'Général - Cours Java', description: "N'oubliez pas le TP de demain.", unread: 3, membersCount: 24, code: 'JAVA-101' },
		{ id: 2, name: 'Projet Final - Web', description: 'Le cahier des charges est en ligne.', unread: 0, membersCount: 18, code: 'WEB-302' },
		{ id: 3, name: 'Algorithmique Avancée', description: "Quelqu'un a compris le tri fusion ?", unread: 12, membersCount: 30, code: 'ALGO-201' },
		{ id: 4, name: 'Base de Données SQL', description: 'La jointure externe est complexe.', unread: 0, membersCount: 22, code: 'BDD-105' },
		{ id: 5, name: 'Intelligence Artificielle', description: 'Nouveau papier sur les LLMs.', unread: 2, membersCount: 40, code: 'IA-400' },
	];

	const archived = [
		{ id: 6, name: 'Projet Année 2023', description: "Archives du projet de l'année dernière.", membersCount: 15, code: 'PRJ-2023' },
		{ id: 7, name: 'Stage Été', description: "Rapports et soutenances.", membersCount: 10, code: 'STG-2023' }
	];

	// Reactive derivation of active channel details
	$: allChannels = [...channels, ...archived];
	$: activeChannel = allChannels.find(c => c.id === activeChannelId) || channels[0];

	// Messages separated per channel
	let messagesData: Record<number, any[]> = {
		1: [
			{ id: 1, sender: 'Prof. Aicha Dakir', role: 'ENSEIGNANT', time: '10:30', text: "Bonjour à tous, j'ai mis à jour le support de cours sur l'héritage en Java.", isTeacher: true, initial: 'A', color: 'bg-green-500' },
			{ id: 2, sender: 'Abdelhadi Ait Boubker', role: '', time: '10:45', text: "Merci Monsieur ! Est-ce que l'examen couvrira aussi les interfaces ?", isTeacher: false, initial: 'A', color: 'bg-yellow-500' },
			{ id: 3, sender: 'Abdelaziz Boukdour', role: '', time: '11:03', text: "J'ai une question sur le polymorphisme, je ne comprends pas bien la liaison dynamique.", isTeacher: false, initial: 'A', color: 'bg-yellow-500' },
			{ id: 4, sender: 'Prof. Aicha Dakir', role: 'ENSEIGNANT', time: '11:15', text: "Très bonne question Abdelhadi. Nous y reviendrons en détail lors de la séance de demain à 14h.", isTeacher: true, initial: 'A', color: 'bg-green-500' },
			{ id: 5, type: 'divider', text: "AUJOURD'HUI" },
			{ id: 6, sender: 'Lahcen ECHCHARIY', role: '', time: 'Il y a 2 min', text: "Est-ce que vous pourriez poster le lien vers le dépôt GitHub du projet ?", isTeacher: false, initial: 'L', color: 'bg-green-500' }
		],
		2: [
			{ id: 101, sender: 'Prof. Youssef EL-SARDY', role: 'ENSEIGNANT', time: '09:00', text: "Le cahier des charges du projet final est maintenant disponible sur la plateforme.", isTeacher: true, initial: 'Y', color: 'bg-green-500' },
			{ id: 102, sender: 'Abdelhadi Ait Boubker', role: '', time: '09:15', text: "C'est noté, merci. La date limite est bien le 30 mai ?", isTeacher: false, initial: 'A', color: 'bg-yellow-500' }
		],
		3: [
			{ id: 201, sender: 'Mourad Amribd', role: '', time: 'Hier', text: "Quelqu'un a compris la complexité du tri fusion dans le pire des cas ?", isTeacher: false, initial: 'M', color: 'bg-blue-300' },
			{ id: 202, sender: 'Lahcen ECHCHARIY', role: '', time: 'Hier', text: "Oui, c'est toujours O(n log n).", isTeacher: false, initial: 'L', color: 'bg-orange-500' }
		],
		4: [
			{ id: 301, sender: 'Hafid Qortali', role: '', time: 'Lundi', text: "Je suis bloqué sur la requête avec le FULL OUTER JOIN, une idée ?", isTeacher: false, initial: 'H', color: 'bg-green-300' }
		],
		5: [
			{ id: 401, sender: 'Abdelhadi Ait Boubker', role: '', time: '08:00', text: "Avez-vous vu le nouveau papier sur les architectures Transformers ?", isTeacher: false, initial: 'A', color: 'bg-yellow-500' },
			{ id: 402, sender: 'Abdelaziz Boukdour', role: '', time: '08:30', text: "Oui incroyable les nouvelles performances sur le raisonnement logique !", isTeacher: false, initial: 'A', color: 'bg-yellow-500' }
		]
	};

	const members = {
		teachers: [
			{ id: 1, name: 'Aicha Dakir', initial: 'A', color: 'bg-green-500' },
			{ id: 2, name: 'Youssef EL-SARDY', initial: 'Y', color: 'bg-green-500' }
		],
		online: [
			{ id: 3, name: 'Abdelhadi Ait Boubker', initial: 'A', color: 'bg-red-500' },
			{ id: 4, name: 'Lahcen ECHCHARIY', initial: 'L', color: 'bg-orange-500' },
			{ id: 5, name: 'Abdelaziz Boukdour', initial: 'A', color: 'bg-green-500' }
		],
		offline: [
			{ id: 6, name: 'Mourad Amribd', initial: 'M', color: 'bg-blue-300' },
			{ id: 7, name: 'Hafid Qortali', initial: 'H', color: 'bg-green-300' }
		]
	};

	// ---- LOGIC & STATE ----

	// Search channels
	let channelSearchQuery = '';
	$: filteredChannels = channels.filter(c => c.name.toLowerCase().includes(channelSearchQuery.toLowerCase()) || c.description.toLowerCase().includes(channelSearchQuery.toLowerCase()));
	$: filteredArchived = archived.filter(c => c.name.toLowerCase().includes(channelSearchQuery.toLowerCase()));

	// Search messages
	let messageSearchQuery = '';
	let showMessageSearch = false;
	
	// Get active messages reactively
	$: currentMessages = messagesData[activeChannelId] || [];
	$: filteredMessages = currentMessages.filter(m => {
		if (messageSearchQuery.trim() === '') return true;
		if (m.type === 'divider') return false; // Hide dividers when searching
		return (m.text && m.text.toLowerCase().includes(messageSearchQuery.toLowerCase())) || 
			   (m.sender && m.sender.toLowerCase().includes(messageSearchQuery.toLowerCase()));
	});

	let newMessageText = '';

	function selectChannel(id: number) {
		activeChannelId = id;
		messageSearchQuery = ''; // Reset message search when switching channels
		showMessageSearch = false; // Hide message search
		
		// Clear unread badge for regular channels
		const channelToUpdate = channels.find(c => c.id === id);
		if (channelToUpdate) channelToUpdate.unread = 0;
        
        // Ensure scroll to bottom on channel switch
        setTimeout(() => {
			const container = document.getElementById('messages-container');
			if (container) {
				container.scrollTop = container.scrollHeight;
			}
		}, 50);
	}

	function sendMessage() {
		if (newMessageText.trim() === '') return;
		
		const newMsg = {
			id: Date.now(),
			sender: 'Moi',
			role: 'ÉLÈVE',
			time: 'À l\'instant',
			text: newMessageText,
			isTeacher: false,
			initial: 'M',
			color: 'bg-blue-500'
		};
		
		// Update exactly the active channel's array
		messagesData[activeChannelId] = [...(messagesData[activeChannelId] || []), newMsg];
		newMessageText = '';

		// Scroll to bottom after DOM updates
		setTimeout(() => {
			const container = document.getElementById('messages-container');
			if (container) {
				container.scrollTop = container.scrollHeight;
			}
		}, 50);
	}

	function handleKeydown(e: KeyboardEvent) {
		if (e.key === 'Enter') {
			sendMessage();
		}
	}
</script>

<div class="flex bg-white dark:bg-gray-800 rounded-3xl shadow-sm border border-gray-100 dark:border-gray-700 h-[calc(100vh-140px)] w-full overflow-hidden">
	
	<!-- Left Column: Channels -->
	<div class="hidden lg:flex w-[320px] flex-shrink-0 border-r border-gray-100 dark:border-gray-700 flex-col bg-white dark:bg-gray-800 transition-colors duration-200">
		<!-- Header -->
		<div class="p-6 pb-4 flex justify-between items-center">
			<h2 class="text-xl font-bold text-gray-800 dark:text-gray-100">Discussions</h2>
			<button class="text-gray-400 hover:text-gray-600 dark:hover:text-gray-200 transition-colors">
				<svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z"></path></svg>
			</button>
		</div>
		<!-- Search -->
		<div class="px-6 mb-6">
			<div class="relative">
				<svg class="absolute left-3 top-2.5 w-4 h-4 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path></svg>
				<input type="text" bind:value={channelSearchQuery} placeholder="Rechercher un canal..." class="w-full bg-gray-50 dark:bg-gray-700 border border-gray-100 dark:border-gray-600 text-sm rounded-xl pl-9 pr-4 py-2 focus:outline-none focus:ring-1 focus:ring-blue-500 dark:text-gray-200 transition-colors" />
			</div>
		</div>
		<!-- Channels List -->
		<div class="flex-1 overflow-y-auto px-4 custom-scrollbar">
			<div class="text-[11px] font-bold text-gray-400 dark:text-gray-500 mb-3 px-2 uppercase tracking-wider">Canaux de cours</div>
			{#each filteredChannels as channel}
				<!-- svelte-ignore a11y-click-events-have-key-events -->
				<!-- svelte-ignore a11y-no-static-element-interactions -->
				<div on:click={() => selectChannel(channel.id)} class="flex items-start p-3 rounded-xl mb-1 cursor-pointer transition-colors {channel.id === activeChannelId ? 'bg-blue-500 text-white shadow-sm' : 'hover:bg-gray-50 dark:hover:bg-gray-700/50 text-gray-700 dark:text-gray-200'}">
					<div class="w-8 h-8 rounded-lg flex items-center justify-center font-bold text-sm shrink-0 {channel.id === activeChannelId ? 'bg-white/20 text-white' : 'bg-gray-100 dark:bg-gray-700 text-gray-400 dark:text-gray-500'}">
						#
					</div>
					<div class="ml-3 flex-1 overflow-hidden">
						<div class="flex justify-between items-center">
							<span class="font-bold text-sm truncate {channel.id === activeChannelId ? 'text-white' : 'text-gray-800 dark:text-gray-200'}">{channel.name}</span>
							{#if channel.unread > 0}
								<span class="bg-red-500 text-white text-[10px] font-bold px-1.5 py-0.5 rounded-full min-w-[20px] text-center shadow-sm">{channel.unread}</span>
							{/if}
						</div>
						<p class="text-xs truncate mt-0.5 {channel.id === activeChannelId ? 'text-blue-100' : 'text-gray-400 dark:text-gray-500'}">{channel.description}</p>
					</div>
				</div>
			{:else}
				<div class="text-xs text-gray-400 px-2 italic mb-4">Aucun canal trouvé</div>
			{/each}

			<div class="text-[11px] font-bold text-gray-400 dark:text-gray-500 mt-6 mb-3 px-2 uppercase tracking-wider">Archivés</div>
			{#each filteredArchived as channel}
				<!-- svelte-ignore a11y-click-events-have-key-events -->
				<!-- svelte-ignore a11y-no-static-element-interactions -->
				<div on:click={() => selectChannel(channel.id)} class="flex items-center p-2 rounded-xl mb-1 cursor-pointer transition-colors {channel.id === activeChannelId ? 'bg-blue-500 text-white shadow-sm' : 'hover:bg-gray-50 dark:hover:bg-gray-700/50 text-gray-500 dark:text-gray-400'}">
					<svg class="w-4 h-4 ml-1 mr-3 {channel.id === activeChannelId ? 'text-white' : 'opacity-60'}" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 20l4-16m2 16l4-16M6 9h14M4 15h14"></path></svg>
					<span class="text-sm font-medium {channel.id === activeChannelId ? 'text-white' : ''}">{channel.name}</span>
				</div>
			{:else}
				<div class="text-xs text-gray-400 px-2 italic">Aucun canal archivé trouvé</div>
			{/each}
		</div>
		<!-- Bottom Preferences -->
		<div class="p-4 border-t border-gray-100 dark:border-gray-700">
			<a href="/student/settings" class="w-full flex items-center p-3 border border-gray-200 dark:border-gray-700 rounded-xl hover:bg-gray-50 dark:hover:bg-gray-700/50 transition-colors">
				<div class="bg-blue-50 dark:bg-blue-900/30 p-1.5 rounded-lg mr-3">
					<svg class="w-4 h-4 text-blue-500 dark:text-blue-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.996.608 2.296.07 2.572-1.065z"></path><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"></path></svg>
				</div>
				<div class="text-left">
					<div class="text-sm font-bold text-gray-800 dark:text-gray-200">Préférences</div>
					<div class="text-[11px] text-gray-400 dark:text-gray-500 mt-0.5">Gérer les notifications</div>
				</div>
			</a>
		</div>
	</div>

	<!-- Center Column: Chat -->
	<div class="flex-1 flex flex-col min-w-0 bg-white dark:bg-gray-800 transition-colors duration-200">
		<!-- Header -->
		<div class="px-6 py-4 border-b border-gray-100 dark:border-gray-700 flex justify-between items-center relative">
			<div class="flex items-center">
				<svg class="w-6 h-6 text-gray-400 mr-3" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 20l4-16m2 16l4-16M6 9h14M4 15h14"></path></svg>
				<div>
					<h2 class="text-lg font-bold text-gray-800 dark:text-gray-100">{activeChannel.name}</h2>
					<div class="text-[11px] text-gray-400 dark:text-gray-500 mt-0.5">{activeChannel.membersCount} membres actifs • {activeChannel.code}</div>
				</div>
			</div>
			<div class="flex items-center space-x-5 text-gray-400">
				<!-- Search toggle button -->
				<button on:click={() => showMessageSearch = !showMessageSearch} class="hover:text-gray-600 dark:hover:text-gray-200 transition-colors {showMessageSearch ? 'text-blue-500' : ''}">
					<svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path></svg>
				</button>
				<button class="hover:text-gray-600 dark:hover:text-gray-200 transition-colors"><svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path></svg></button>
				<button class="hover:text-gray-600 dark:hover:text-gray-200 transition-colors"><svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg></button>
			</div>
		</div>

		<!-- Conditional Message Search Bar -->
		{#if showMessageSearch}
		<div class="bg-gray-50 dark:bg-gray-800/50 px-6 py-3 border-b border-gray-100 dark:border-gray-700 animate-in slide-in-from-top-2">
			<div class="relative">
				<svg class="absolute left-3 top-2.5 w-4 h-4 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path></svg>
				<input type="text" bind:value={messageSearchQuery} placeholder="Rechercher dans cette conversation..." class="w-full bg-white dark:bg-gray-700 border border-gray-200 dark:border-gray-600 text-sm rounded-lg pl-9 pr-4 py-2 focus:outline-none focus:ring-1 focus:ring-blue-500 dark:text-gray-200 transition-colors" autofocus />
			</div>
		</div>
		{/if}
		
		<!-- Banner -->
		{#if activeChannelId === 1}
		<div class="bg-blue-50 dark:bg-blue-900/20 px-6 py-2.5 flex items-center justify-between text-sm">
			<div class="flex items-center text-blue-600 dark:text-blue-400 min-w-0">
				<svg class="w-4 h-4 mr-2 shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9"></path></svg>
				<span class="font-medium truncate text-xs sm:text-sm">Rappel : Devoir de l'Intelligence Artificielle à rendre demain avant 23h59.</span>
			</div>
			<button class="text-blue-600 dark:text-blue-400 font-bold hover:underline shrink-0 ml-4 text-xs sm:text-sm">Voir détails</button>
		</div>
		{/if}

		<!-- Messages Area -->
		<div id="messages-container" class="flex-1 overflow-y-auto p-6 space-y-6 custom-scrollbar scroll-smooth">
			<!-- Debut conversation -->
			{#if messageSearchQuery.trim() === '' && filteredMessages.length > 0}
				<div class="bg-purple-200/50 dark:bg-purple-900/30 rounded-2xl p-10 text-center mb-8 mx-4">
					<div class="text-[11px] font-bold text-purple-700 dark:text-purple-400 uppercase tracking-[0.15em]">Début de la conversation dans #{activeChannel.name}</div>
				</div>
			{/if}

			{#each filteredMessages as msg}
				{#if msg.type === 'divider'}
					<div class="flex items-center justify-center my-6">
						<div class="h-px bg-gray-100 dark:bg-gray-700 flex-1"></div>
						<span class="px-4 text-[10px] font-bold text-gray-400 dark:text-gray-500 uppercase tracking-wider">{msg.text}</span>
						<div class="h-px bg-gray-100 dark:bg-gray-700 flex-1"></div>
					</div>
				{:else}
					<div class="flex items-start">
						<!-- Avatar -->
						<div class="w-10 h-10 rounded-full flex items-center justify-center text-white font-bold shrink-0 {msg.color} shadow-sm">
							{msg.initial}
						</div>
						<div class="ml-4 flex-1">
							<div class="flex items-center mb-1.5">
								<span class="font-bold text-[15px] text-gray-800 dark:text-gray-200 mr-2">{msg.sender}</span>
								{#if msg.role}
									<span class="bg-blue-500 text-white text-[9px] font-bold px-2 py-0.5 rounded-[4px] mr-2 tracking-wide uppercase">{msg.role}</span>
								{/if}
								<span class="text-[11px] font-medium text-gray-400">{msg.time}</span>
							</div>
							<div class="inline-block px-5 py-3 rounded-2xl rounded-tl-sm text-sm text-gray-700 dark:text-gray-200 {msg.isTeacher ? 'bg-[#e8f0fe] dark:bg-blue-900/40' : 'bg-gray-50 dark:bg-gray-700/50 border border-gray-100 dark:border-gray-600'}">
								{msg.text}
							</div>
						</div>
					</div>
				{/if}
			{:else}
				<div class="flex flex-col items-center justify-center h-full text-gray-400 dark:text-gray-500 font-medium">
					<svg class="w-12 h-12 mb-3 text-gray-300 dark:text-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z"></path></svg>
					{#if messageSearchQuery.trim() !== ''}
						Aucun message ne correspond à "{messageSearchQuery}".
					{:else}
						C'est calme ici ! Soyez le premier à envoyer un message.
					{/if}
				</div>
			{/each}
		</div>

		<!-- Input Area -->
		<div class="p-6 pt-2">
			<div class="border border-gray-200 dark:border-gray-700 rounded-[20px] shadow-sm bg-white dark:bg-gray-800 focus-within:ring-2 focus-within:ring-blue-500 focus-within:border-transparent transition-all overflow-hidden">
				<input type="text" bind:value={newMessageText} on:keydown={handleKeydown} placeholder="Envoyer un message dans #{activeChannel.name}" class="w-full bg-transparent border-none text-sm px-5 py-3.5 focus:outline-none focus:ring-0 dark:text-gray-200 placeholder-gray-400" />
				
				<div class="flex items-center justify-between px-3 py-2.5 border-t border-gray-100 dark:border-gray-700">
					<div class="flex items-center space-x-1 text-gray-400 dark:text-gray-500">
						<button class="p-2 hover:bg-gray-100 dark:hover:bg-gray-700 rounded-lg transition-colors"><svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.172 7l-6.586 6.586a2 2 0 102.828 2.828l6.414-6.586a4 4 0 00-5.656-5.656l-6.415 6.585a6 6 0 108.486 8.486L20.5 13"></path></svg></button>
						<button class="p-2 hover:bg-gray-100 dark:hover:bg-gray-700 rounded-lg transition-colors"><svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14.828 14.828a4 4 0 01-5.656 0M9 10h.01M15 10h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg></button>
						<div class="w-px h-4 bg-gray-200 dark:bg-gray-600 mx-1"></div>
						<button class="p-2 hover:bg-gray-100 dark:hover:bg-gray-700 rounded-lg transition-colors font-bold font-serif text-[15px]">B</button>
						<button class="p-2 hover:bg-gray-100 dark:hover:bg-gray-700 rounded-lg transition-colors italic font-serif text-[15px]">I</button>
					</div>
					
					<div class="flex items-center pr-1">
						<span class="text-xs text-gray-400 dark:text-gray-500 mr-4 font-medium hidden sm:inline-block">Entrée pour envoyer</span>
						<button on:click={sendMessage} class="bg-blue-600 hover:bg-blue-700 text-white font-bold text-sm px-5 py-2 rounded-xl flex items-center transition-colors shadow-sm">
							Envoyer
							<svg class="w-3.5 h-3.5 ml-2" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 19l9 2-9-18-9 18 9-2zm0 0v-8"></path></svg>
						</button>
					</div>
				</div>
			</div>
		</div>
	</div>

	<!-- Right Column: Members -->
	<div class="hidden xl:flex w-[280px] flex-shrink-0 border-l border-gray-100 dark:border-gray-700 flex-col bg-white dark:bg-gray-800 transition-colors duration-200">
		<div class="p-6 border-b border-gray-100 dark:border-gray-700 flex justify-between items-center">
			<h2 class="font-bold text-[15px] text-gray-800 dark:text-gray-100">Membres du canal</h2>
			<span class="text-[11px] text-gray-400 font-bold">{activeChannel.membersCount}</span>
		</div>
		
		<div class="flex-1 overflow-y-auto p-6 pt-5 custom-scrollbar">
			<!-- Teachers -->
			<div class="text-[10px] font-bold text-gray-400 dark:text-gray-500 mb-4 uppercase tracking-wider">Enseignants</div>
			{#each members.teachers as teacher}
				<div class="flex items-center mb-4 cursor-pointer group">
					<div class="relative">
						<div class="w-8 h-8 rounded-full flex items-center justify-center text-white font-bold text-[13px] {teacher.color} shadow-sm transition-transform group-hover:scale-105">
							{teacher.initial}
						</div>
						<div class="absolute bottom-0 right-0 w-2.5 h-2.5 bg-green-500 border-2 border-white dark:border-gray-800 rounded-full"></div>
					</div>
					<span class="ml-3 text-[13px] font-bold text-gray-700 dark:text-gray-300 group-hover:text-blue-500 transition-colors">{teacher.name}</span>
				</div>
			{/each}

			<!-- Online -->
			<div class="text-[10px] font-bold text-gray-400 dark:text-gray-500 mt-8 mb-4 uppercase tracking-wider">En ligne — {members.online.length}</div>
			{#each members.online as user}
				<div class="flex items-center mb-4 cursor-pointer group">
					<div class="relative">
						<div class="w-8 h-8 rounded-full flex items-center justify-center text-white font-bold text-[13px] {user.color} shadow-sm transition-transform group-hover:scale-105">
							{user.initial}
						</div>
						<div class="absolute bottom-0 right-0 w-2.5 h-2.5 bg-green-500 border-2 border-white dark:border-gray-800 rounded-full"></div>
					</div>
					<span class="ml-3 text-[13px] font-medium text-gray-600 dark:text-gray-400 group-hover:text-blue-500 transition-colors">{user.name}</span>
				</div>
			{/each}

			<!-- Offline -->
			<div class="text-[10px] font-bold text-gray-400 dark:text-gray-500 mt-8 mb-4 uppercase tracking-wider">Hors ligne — {activeChannel.membersCount - members.teachers.length - members.online.length}</div>
			{#each members.offline as user}
				<div class="flex items-center mb-4 opacity-60 cursor-pointer group hover:opacity-100 transition-opacity">
					<div class="w-8 h-8 rounded-full flex items-center justify-center text-white font-bold text-[13px] {user.color} shadow-sm transition-transform group-hover:scale-105">
						{user.initial}
					</div>
					<span class="ml-3 text-[13px] font-medium text-gray-600 dark:text-gray-400 group-hover:text-blue-500 transition-colors">{user.name}</span>
				</div>
			{/each}
		</div>

		<!-- Invite Button -->
		<div class="p-4 border-t border-gray-100 dark:border-gray-700">
			<button class="w-full flex items-center justify-center py-3 border border-gray-200 dark:border-gray-700 rounded-[14px] hover:bg-gray-50 dark:hover:bg-gray-700/50 text-gray-700 dark:text-gray-300 font-bold text-sm transition-colors shadow-sm">
				<svg class="w-4 h-4 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M18 9v3m0 0v3m0-3h3m-3 0h-3m-2-5a4 4 0 11-8 0 4 4 0 018 0zM3 20a6 6 0 0112 0v1H3v-1z"></path></svg>
				Inviter des membres
			</button>
		</div>
	</div>
</div>

<style>
	/* Optional: Custom thin scrollbar for areas */
	.custom-scrollbar::-webkit-scrollbar {
		width: 4px;
	}
	.custom-scrollbar::-webkit-scrollbar-track {
		background: transparent;
	}
	.custom-scrollbar::-webkit-scrollbar-thumb {
		background-color: rgba(156, 163, 175, 0.3);
		border-radius: 20px;
	}
	:global(.dark) .custom-scrollbar::-webkit-scrollbar-thumb {
		background-color: rgba(75, 85, 99, 0.5);
	}
</style>
