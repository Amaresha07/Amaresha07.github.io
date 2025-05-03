<script>
	import { onMount } from 'svelte';
	import { fade, fly, scale } from 'svelte/transition';

	// Navigation links
	const navLinks = [
		{ name: 'Home', href: '#home' },
		{ name: 'About', href: '#about' },
		{ name: 'Skills', href: '#skills' },
		{ name: 'Projects', href: '#projects' },
		{ name: 'Education', href: '#education' },
		{ name: 'Activities', href: '#activities' },
		{ name: 'Contact', href: '#contact' }
	];

	// Skills data with categories
	const skills = [
		{ name: 'GO', level: 85, category: 'Languages', details: 'Goroutines, Channels, Interfaces' },
		{ name: 'C & DS', level: 80, category: 'Languages' },
		{ name: 'JAVA', level: 65, category: 'Languages', details: 'Basics' },
		{ name: 'SvelteKit', level: 90, category: 'Frontend' },
		{ name: 'PostgreSQL', level: 85, category: 'Database' },
		{ name: 'Git', level: 75, category: 'Tools' },
		{ name: 'Linux', level: 80, category: 'Technologies' }
	];

	// Projects data
	const projects = [
		{
			title: 'Coffee Vending Machine',
			period: 'July 2024 – Dec 2024',
			technologies: [
				'Arduino',
				'IR Sensors',
				'Servo Motor',
				'RFID',
				'WaterPump',
				'MotorDriver',
				'UltraSonic Sensor'
			],
			description: [
				'Developed a fully automated coffee vending machine that provides freshly brewed coffee with customizable options.',
				'Supports cashless transactions through RFID, QR codes, and mobile wallets, making payments more convenient and secure.',
				'Includes real-time inventory tracking and automated maintenance alerts to ensure smooth operation and hygiene.',
				'Can be installed in workplaces, colleges, airports, hospitals, and shopping malls, providing quick access to coffee in busy environments.',
				'Enhances user experience with a touchscreen interface, allowing easy selection of coffee types and customization options.'
			]
		},
		{
			title: 'Library Management System',
			period: 'May 2024',
			technologies: ['GOlang', 'SvelteKit', 'PostgreSQL'],
			description: [
				'Built frontend using Svelte with TypeScript, featuring a responsive UI, book search, and borrowing management.',
				'Developed backend in Go (Golang), handling user authentication, book inventory, and borrowing/return operations.',
				'Implemented PostgreSQL database to store user details, book records, and transaction history for efficient data management.',
				'Supported role-based access, allowing admins to manage books while users can borrow and return them securely.'
			]
		},
		{
			title: 'Bank Transaction Management System',
			period: '2024',
			technologies: ['Golang', 'REST API', 'PostgreSQL', 'HTTP'],
			description: [
				'Developed a secure bank transaction system using Go and PostgreSQL, designing backend logic to handle user transactions efficiently.',
				'Implemented user authentication, deposits, withdrawals, and balance updates, ensuring secure login and smooth transaction processing.',
				'Ensured data integrity with PostgreSQL transactions (COMMIT, ROLLBACK), preventing inconsistencies by handling failures with rollback mechanisms.',
				'Designed RESTful APIs for handling user transactions and history retrieval, providing structured API endpoints for seamless data access and interaction.'
			]
		}
	];

	// Activities data
	const activities = [
		{
			title: 'Volunteer',
			organization: "International Cultural Jamboree, Alva's Institution",
			year: '2023',
			description: [
				'Volunteered at the International Cultural Jamboree, participating with over 50,000 students from India and 13 foreign countries, showcasing leadership and teamwork in a diverse environment.',
				'Demonstrated effective leadership abilities while coordinating with a large group of participants and organizing cultural events.'
			]
		},
		{
			title: 'State-level Weightlifting Player',
			organization: 'VTU Champions Team',
			year: '2023',
			description: [
				'Gold Medalist in VTU Weightlifting Championship (State Level). Participated in National Level Competition.',
				'Developed strong teamwork, discipline, and physical endurance through competitive Weightlifting.'
			]
		}
	];

	// Mobile menu state
	let mobileMenuOpen = false;
	let scrollY;
	let activeSection = 'home';
	let sections = [];
	let isScrolling = false;

	// Handle scroll to update active section
	function handleScroll() {
		if (!isScrolling) {
			isScrolling = true;

			setTimeout(() => {
				const currentPosition = scrollY + 100;

				for (let i = sections.length - 1; i >= 0; i--) {
					const section = sections[i];
					if (section && currentPosition >= section.offsetTop) {
						activeSection = section.id;
						break;
					}
				}
				isScrolling = false;
			}, 100);
		}
	}

	// Smooth scroll to section
	function scrollToSection(id) {
		const element = document.getElementById(id);
		if (element) {
			window.scrollTo({
				top: element.offsetTop - 80,
				behavior: 'smooth'
			});
		}
		mobileMenuOpen = false;
	}

	onMount(() => {
		sections = Array.from(document.querySelectorAll('section[id]'));
	});

	// Form handling
	let name = '';
	let email = '';
	let message = '';
	let formSubmitted = false;

	function handleSubmit() {
		// In a real application, you would send this data to a server
		console.log({ name, email, message });
		formSubmitted = true;

		// Reset form after submission
		setTimeout(() => {
			name = '';
			email = '';
			message = '';
			formSubmitted = false;
		}, 3000);
	}
</script>

<svelte:window bind:scrollY on:scroll={handleScroll} />

<div class="min-h-screen bg-gradient-to-br from-gray-50 to-white font-sans text-gray-900">
	<!-- Header -->
	<header
		class="fixed top-0 right-0 left-0 z-50 bg-white/90 shadow-sm backdrop-blur-sm transition-all duration-300"
	>
		<div class="container mx-auto flex items-center justify-between px-4 py-4">
			<a href="#home" class="flex items-center text-2xl font-bold text-emerald-600">
				<span
					class="mr-2 flex h-10 w-10 items-center justify-center rounded-full bg-emerald-600 text-white"
					>AM</span
				>
				<span class="hidden sm:inline">Amaresha M</span>
			</a>

			<!-- Desktop Navigation -->
			<nav class="hidden space-x-8 md:flex">
				{#each navLinks as link}
					<a
						href={link.href}
						class="relative py-2 text-gray-700 transition-colors duration-300 hover:text-emerald-600 {activeSection ===
						link.href.substring(1)
							? 'font-medium text-emerald-600'
							: ''}"
						on:click|preventDefault={() => scrollToSection(link.href.substring(1))}
					>
						{link.name}
						{#if activeSection === link.href.substring(1)}
							<span class="absolute -bottom-1 left-0 h-0.5 w-full rounded-full bg-emerald-600"
							></span>
						{/if}
					</a>
				{/each}
			</nav>

			<!-- Mobile Menu Button -->
			<button
				class="text-gray-700 focus:outline-none md:hidden"
				on:click={() => (mobileMenuOpen = !mobileMenuOpen)}
				aria-label="Toggle menu"
			>
				<svg
					xmlns="http://www.w3.org/2000/svg"
					class="h-6 w-6"
					fill="none"
					viewBox="0 0 24 24"
					stroke="currentColor"
				>
					{#if mobileMenuOpen}
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width="2"
							d="M6 18L18 6M6 6l12 12"
						/>
					{:else}
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width="2"
							d="M4 6h16M4 12h16M4 18h16"
						/>
					{/if}
				</svg>
			</button>
		</div>

		<!-- Mobile Navigation -->
		{#if mobileMenuOpen}
			<div
				class="absolute w-full bg-white px-6 py-4 shadow-lg md:hidden"
				transition:fly={{ y: -20, duration: 300 }}
			>
				{#each navLinks as link}
					<a
						href={link.href}
						class="block py-2 text-gray-700 transition-colors duration-300 hover:text-emerald-600 {activeSection ===
						link.href.substring(1)
							? 'font-medium text-emerald-600'
							: ''}"
						on:click|preventDefault={() => scrollToSection(link.href.substring(1))}
					>
						{link.name}
					</a>
				{/each}
			</div>
		{/if}
	</header>

	<main class="pt-20">
		<!-- Hero Section -->
		<section
			id="home"
			class="relative flex min-h-screen items-center justify-center overflow-hidden bg-gradient-to-br from-emerald-50 to-teal-100 py-20"
		>
			<!-- Background Pattern -->
			<div
				class="absolute inset-0 bg-[url('data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNjAiIGhlaWdodD0iNjAiIHZpZXdCb3g9IjAgMCA2MCA2MCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48ZyBmaWxsPSJub25lIiBmaWxsLXJ1bGU9ImV2ZW5vZGQiPjxnIGZpbGw9IiMwMDk2ODgiIGZpbGwtb3BhY2l0eT0iMC4wMyI+PHBhdGggZD0iTTM2IDM0aDR2MWgtNHYtMXptMC0yaDF2NGgtMXYtNHptMi0yaDF2MWgtMXYtMXptLTIgMmgxdjFoLTF2LTF6bS0yLTJoMXYxaC0xdi0xem0yLTJoMXYxaC0xdi0xem0tMiAyaDF2MWgtMXYtMXptLTItMmgxdjFoLTF2LTF6Ii8+PC9nPjwvZz48L3N2Zz4=')] opacity-50"
			></div>

			<!-- Animated Circles -->
			<div
				class="animate-blob absolute top-1/4 left-1/4 h-64 w-64 rounded-full bg-emerald-300 opacity-20 mix-blend-multiply blur-3xl filter"
			></div>
			<div
				class="animate-blob animation-delay-2000 absolute top-1/3 right-1/4 h-64 w-64 rounded-full bg-teal-300 opacity-20 mix-blend-multiply blur-3xl filter"
			></div>
			<div
				class="animate-blob animation-delay-4000 absolute right-1/3 bottom-1/4 h-64 w-64 rounded-full bg-cyan-300 opacity-20 mix-blend-multiply blur-3xl filter"
			></div>

			<div
				class="relative z-10 container mx-auto flex flex-col-reverse items-center justify-between px-4 md:flex-row"
			>
				<div class="mt-10 md:mt-0 md:w-1/2" in:fade={{ duration: 1000, delay: 300 }}>
					<span
						class="mb-6 inline-block rounded-full bg-emerald-100 px-4 py-1 text-sm font-medium text-emerald-800"
					>
						Electronics & Communication Engineer
					</span>
					<h1 class="mb-4 text-4xl font-bold text-gray-900 md:text-5xl lg:text-6xl">
						Hi, I'm <span class="text-emerald-600">Amaresha M</span>
					</h1>
					<p class="mb-8 max-w-lg text-lg text-gray-600">
						Passionate about building innovative solutions with Go, SvelteKit, and PostgreSQL.
						Focused on creating efficient and user-friendly applications.
					</p>
					<div class="flex flex-wrap gap-4">
						<a
							href="#contact"
							class="transform rounded-lg bg-emerald-600 px-6 py-3 text-white shadow-lg transition-all duration-300 hover:scale-105 hover:bg-emerald-700 hover:shadow-xl"
							on:click|preventDefault={() => scrollToSection('contact')}
						>
							Contact Me
						</a>
						<a
							href="#projects"
							class="transform rounded-lg border border-emerald-600 bg-white px-6 py-3 text-emerald-600 shadow-lg transition-all duration-300 hover:scale-105 hover:bg-emerald-50 hover:shadow-xl"
							on:click|preventDefault={() => scrollToSection('projects')}
						>
							View Projects
						</a>
					</div>
				</div>
				<div class="flex justify-center md:w-1/2" in:fade={{ duration: 1000 }}>
					<div class="relative">
						<div
							class="absolute inset-0 scale-110 transform rounded-full bg-emerald-600 opacity-20 blur-md"
						></div>
						<div
							class="relative z-10 h-64 w-64 transform overflow-hidden rounded-full border-4 border-white shadow-xl transition-transform duration-300 hover:scale-105 md:h-80 md:w-80"
						>
							<img
								src="https://hebbkx1anhila5yf.public.blob.vercel-storage.com/amaresh-pkhd0HW2iYWsFn7rFir39v9Y1jifQt.jpeg"
								alt="Amaresha M"
								class="h-full w-full object-cover"
							/>
						</div>
					</div>
				</div>
			</div>

			<div class="absolute bottom-10 left-1/2 -translate-x-1/2 transform animate-bounce">
				<a
					href="#about"
					on:click|preventDefault={() => scrollToSection('about')}
					class="text-emerald-600 transition-colors duration-300 hover:text-emerald-700"
				>
					<svg
						xmlns="http://www.w3.org/2000/svg"
						class="h-8 w-8"
						fill="none"
						viewBox="0 0 24 24"
						stroke="currentColor"
					>
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width="2"
							d="M19 14l-7 7m0 0l-7-7m7 7V3"
						/>
					</svg>
				</a>
			</div>
		</section>

		<!-- About Section -->
		<section id="about" class="bg-white py-20">
			<div class="container mx-auto px-4">
				<h2 class="relative mb-16 text-center text-3xl font-bold md:text-4xl">
					About Me
					<span class="mx-auto mt-4 block h-1 w-20 bg-emerald-600"></span>
				</h2>

				<div class="flex flex-col items-center justify-between gap-12 md:flex-row">
					<div class="md:w-2/5">
						<div
							class="transform rounded-2xl bg-gradient-to-br from-emerald-50 to-teal-50 p-8 shadow-lg transition-transform duration-300 hover:scale-105"
						>
							<h3 class="mb-6 flex items-center text-2xl font-semibold text-emerald-600">
								<svg
									xmlns="http://www.w3.org/2000/svg"
									class="mr-2 h-6 w-6"
									fill="none"
									viewBox="0 0 24 24"
									stroke="currentColor"
								>
									<path
										stroke-linecap="round"
										stroke-linejoin="round"
										stroke-width="2"
										d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"
									/>
								</svg>
								Personal Info
							</h3>
							<ul class="space-y-6">
								<li class="flex items-start">
									<div class="mr-4 flex-shrink-0 rounded-full bg-white p-2 shadow-md">
										<svg
											xmlns="http://www.w3.org/2000/svg"
											class="h-5 w-5 text-emerald-600"
											fill="none"
											viewBox="0 0 24 24"
											stroke="currentColor"
										>
											<path
												stroke-linecap="round"
												stroke-linejoin="round"
												stroke-width="2"
												d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"
											/>
											<path
												stroke-linecap="round"
												stroke-linejoin="round"
												stroke-width="2"
												d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"
											/>
										</svg>
									</div>
									<div>
										<span class="font-medium text-gray-800">Location:</span>
										<p class="text-gray-600">Karnataka, Raichur, Uppala</p>
									</div>
								</li>
								<li class="flex items-start">
									<div class="mr-4 flex-shrink-0 rounded-full bg-white p-2 shadow-md">
										<svg
											xmlns="http://www.w3.org/2000/svg"
											class="h-5 w-5 text-emerald-600"
											fill="none"
											viewBox="0 0 24 24"
											stroke="currentColor"
										>
											<path
												stroke-linecap="round"
												stroke-linejoin="round"
												stroke-width="2"
												d="M3 5a2 2 0 012-2h3.28a1 1 0 01.948.684l1.498 4.493a1 1 0 01-.502 1.21l-2.257 1.13a11.042 11.042 0 005.516 5.516l1.13-2.257a1 1 0 011.21-.502l4.493 1.498a1 1 0 01.684.949V19a2 2 0 01-2 2h-1C9.716 21 3 14.284 3 6V5z"
											/>
										</svg>
									</div>
									<div>
										<span class="font-medium text-gray-800">Phone:</span>
										<p class="text-gray-600">8310388742</p>
									</div>
								</li>
								<li class="flex items-start">
									<div class="mr-4 flex-shrink-0 rounded-full bg-white p-2 shadow-md">
										<svg
											xmlns="http://www.w3.org/2000/svg"
											class="h-5 w-5 text-emerald-600"
											fill="none"
											viewBox="0 0 24 24"
											stroke="currentColor"
										>
											<path
												stroke-linecap="round"
												stroke-linejoin="round"
												stroke-width="2"
												d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"
											/>
										</svg>
									</div>
									<div>
										<span class="font-medium text-gray-800">Email:</span>
										<p class="text-gray-600">amaresham50@gmail.com</p>
									</div>
								</li>
								<li class="flex items-start">
									<div class="mr-4 flex-shrink-0 rounded-full bg-white p-2 shadow-md">
										<svg
											xmlns="http://www.w3.org/2000/svg"
											class="h-5 w-5 text-emerald-600"
											fill="none"
											viewBox="0 0 24 24"
											stroke="currentColor"
										>
											<path
												stroke-linecap="round"
												stroke-linejoin="round"
												stroke-width="2"
												d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14"
											/>
										</svg>
									</div>
									<div>
										<span class="font-medium text-gray-800">LinkedIn:</span>
										<p class="text-gray-600">linkedin.com/in/amaresha-m-44b868249</p>
									</div>
								</li>
								<li class="flex items-start">
									<div class="mr-4 flex-shrink-0 rounded-full bg-white p-2 shadow-md">
										<svg
											xmlns="http://www.w3.org/2000/svg"
											class="h-5 w-5 text-emerald-600"
											fill="none"
											viewBox="0 0 24 24"
											stroke="currentColor"
										>
											<path
												stroke-linecap="round"
												stroke-linejoin="round"
												stroke-width="2"
												d="M10 20l4-16m4 4l4 4-4 4M6 16l-4-4 4-4"
											/>
										</svg>
									</div>
									<div>
										<span class="font-medium text-gray-800">GitHub:</span>
										<p class="text-gray-600">github.com/Amaresha07</p>
									</div>
								</li>
							</ul>
						</div>
					</div>

					<div class="md:w-1/2">
						<h3 class="mb-6 text-2xl font-semibold text-gray-800">Who Am I?</h3>
						<p class="mb-6 text-lg leading-relaxed text-gray-600">
							I'm an Electronics and Communication Engineering student with a passion for software
							development. I specialize in backend development with Go, frontend development with
							SvelteKit, and database management with PostgreSQL.
						</p>
						<p class="mb-6 text-lg leading-relaxed text-gray-600">
							My technical journey has led me to develop various projects, from automated coffee
							vending machines to library management systems and secure banking applications. I
							enjoy solving complex problems and creating efficient, user-friendly solutions.
						</p>
						<p class="text-lg leading-relaxed text-gray-600">
							Beyond coding, I'm an accomplished weightlifter and enjoy volunteering for cultural
							events. I believe in continuous learning and am always looking to expand my skills and
							take on new challenges.
						</p>

						<div class="mt-8 flex flex-wrap gap-3">
							<span
								class="rounded-full bg-emerald-100 px-4 py-2 text-sm font-medium text-emerald-700"
								>Go</span
							>
							<span
								class="rounded-full bg-emerald-100 px-4 py-2 text-sm font-medium text-emerald-700"
								>SvelteKit</span
							>
							<span
								class="rounded-full bg-emerald-100 px-4 py-2 text-sm font-medium text-emerald-700"
								>PostgreSQL</span
							>
							<span
								class="rounded-full bg-emerald-100 px-4 py-2 text-sm font-medium text-emerald-700"
								>C</span
							>
							<span
								class="rounded-full bg-emerald-100 px-4 py-2 text-sm font-medium text-emerald-700"
								>Java</span
							>
							<span
								class="rounded-full bg-emerald-100 px-4 py-2 text-sm font-medium text-emerald-700"
								>Git</span
							>
							<span
								class="rounded-full bg-emerald-100 px-4 py-2 text-sm font-medium text-emerald-700"
								>Linux</span
							>
						</div>
					</div>
				</div>
			</div>
		</section>

		<!-- Skills Section -->
		<section
			id="skills"
			class="relative overflow-hidden bg-gradient-to-br from-gray-50 to-emerald-50 py-20"
		>
			<!-- Background Pattern -->
			<div
				class="absolute inset-0 bg-[url('data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNjAiIGhlaWdodD0iNjAiIHZpZXdCb3g9IjAgMCA2MCA2MCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48ZyBmaWxsPSJub25lIiBmaWxsLXJ1bGU9ImV2ZW5vZGQiPjxnIGZpbGw9IiMwMDk2ODgiIGZpbGwtb3BhY2l0eT0iMC4wMyI+PHBhdGggZD0iTTM2IDM0aDR2MWgtNHYtMXptMC0yaDF2NGgtMXYtNHptMi0yaDF2MWgtMXYtMXptLTIgMmgxdjFoLTF2LTF6bS0yLTJoMXYxaC0xdi0xem0yLTJoMXYxaC0xdi0xem0tMiAyaDF2MWgtMXYtMXptLTItMmgxdjFoLTF2LTF6Ii8+PC9nPjwvZz48L3N2Zz4=')] opacity-50"
			></div>

			<div class="relative z-10 container mx-auto px-4">
				<h2 class="relative mb-16 text-center text-3xl font-bold md:text-4xl">
					Technical Skills
					<span class="mx-auto mt-4 block h-1 w-20 bg-emerald-600"></span>
				</h2>

				<div class="grid grid-cols-1 gap-8 md:grid-cols-2 lg:grid-cols-3">
					{#each skills as skill, i}
						<div
							class="transform rounded-xl border-b-4 border-emerald-500 bg-white p-8 shadow-md transition-all duration-300 hover:scale-105 hover:shadow-xl"
							in:fly={{ y: 50, duration: 500, delay: i * 100 }}
						>
							<div class="mb-4 flex items-center justify-between">
								<h3 class="text-xl font-semibold text-emerald-600">{skill.name}</h3>
								<span class="text-lg font-bold text-emerald-600">{skill.level}%</span>
							</div>

							{#if skill.details}
								<p class="mb-4 text-sm text-gray-600">{skill.details}</p>
							{/if}

							<div class="mb-4 h-2.5 w-full rounded-full bg-gray-200">
								<div class="h-2.5 rounded-full bg-emerald-600" style="width: {skill.level}%"></div>
							</div>

							<div class="flex items-center">
								<span
									class="inline-block rounded-full bg-emerald-100 px-3 py-1 text-xs font-medium text-emerald-700"
								>
									{skill.category}
								</span>
							</div>
						</div>
					{/each}
				</div>
			</div>
		</section>

		<!-- Projects Section -->
		<section id="projects" class="bg-white py-20">
			<div class="container mx-auto px-4">
				<h2 class="relative mb-16 text-center text-3xl font-bold md:text-4xl">
					Projects
					<span class="mx-auto mt-4 block h-1 w-20 bg-emerald-600"></span>
				</h2>

				<div class="space-y-16">
					{#each projects as project, i}
						<div
							class="transform rounded-2xl bg-gradient-to-br from-white to-emerald-50 p-8 shadow-lg transition-all duration-500 hover:shadow-xl"
							in:fly={{ x: i % 2 === 0 ? -50 : 50, duration: 500, delay: i * 150 }}
						>
							<div class="flex flex-col gap-6">
								<div class="flex flex-col justify-between gap-4 md:flex-row md:items-center">
									<h3 class="text-2xl font-bold text-emerald-600">{project.title}</h3>
									<span
										class="inline-block rounded-full bg-emerald-100 px-4 py-1 text-sm font-medium text-emerald-700"
									>
										{project.period}
									</span>
								</div>

								<div>
									<h4 class="mb-3 text-sm font-medium text-gray-700">Technologies:</h4>
									<div class="flex flex-wrap gap-2">
										{#each project.technologies as tech}
											<span class="rounded-full bg-gray-100 px-3 py-1 text-xs text-gray-700"
												>{tech}</span
											>
										{/each}
									</div>
								</div>

								<div>
									<h4 class="mb-3 text-sm font-medium text-gray-700">Key Features:</h4>
									<ul class="mb-6 space-y-3">
										{#each project.description as point}
											<li class="flex items-start">
												<svg
													xmlns="http://www.w3.org/2000/svg"
													class="mt-0.5 mr-2 h-5 w-5 flex-shrink-0 text-emerald-500"
													viewBox="0 0 20 20"
													fill="currentColor"
												>
													<path
														fill-rule="evenodd"
														d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z"
														clip-rule="evenodd"
													/>
												</svg>
												<span class="text-gray-600">{point}</span>
											</li>
										{/each}
									</ul>
								</div>
							</div>
						</div>
					{/each}
				</div>
			</div>
		</section>

		<!-- Education Section -->
		<section
			id="education"
			class="relative overflow-hidden bg-gradient-to-br from-gray-50 to-emerald-50 py-20"
		>
			<!-- Background Pattern -->
			<div
				class="absolute inset-0 bg-[url('data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNjAiIGhlaWdodD0iNjAiIHZpZXdCb3g9IjAgMCA2MCA2MCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48ZyBmaWxsPSJub25lIiBmaWxsLXJ1bGU9ImV2ZW5vZGQiPjxnIGZpbGw9IiMwMDk2ODgiIGZpbGwtb3BhY2l0eT0iMC4wMyI+PHBhdGggZD0iTTM2IDM0aDR2MWgtNHYtMXptMC0yaDF2NGgtMXYtNHptMi0yaDF2MWgtMXYtMXptLTIgMmgxdjFoLTF2LTF6bS0yLTJoMXYxaC0xdi0xem0yLTJoMXYxaC0xdi0xem0tMiAyaDF2MWgtMXYtMXptLTItMmgxdjFoLTF2LTF6Ii8+PC9nPjwvZz48L3N2Zz4=')] opacity-30"
			></div>

			<div class="relative z-10 container mx-auto px-4">
				<h2 class="relative mb-16 text-center text-3xl font-bold md:text-4xl">
					Education
					<span class="mx-auto mt-4 block h-1 w-20 bg-emerald-600"></span>
				</h2>

				<div
					class="mx-auto max-w-3xl transform rounded-2xl bg-white p-8 shadow-lg transition-transform duration-300 hover:scale-105"
				>
					<div class="mb-6 flex flex-col justify-between md:flex-row md:items-center">
						<div>
							<h3 class="text-2xl font-bold text-emerald-600">Bachelor of Engineering (B.E)</h3>
							<p class="text-lg text-gray-700">Electronics and Communication Engineering</p>
						</div>
						<div class="mt-2 md:mt-0">
							<span
								class="rounded-full bg-emerald-100 px-3 py-1 text-sm font-medium text-emerald-800"
							>
								Nov 2021 - April 2025
							</span>
						</div>
					</div>

					<div class="mb-6 flex flex-col items-start justify-between md:flex-row md:items-center">
						<p class="text-gray-600">Visvesvaraya Technological University (VTU)</p>
						<p class="text-gray-600">Mudubidre, Karnataka</p>
					</div>

					<div class="flex items-center rounded-lg bg-emerald-50 p-6">
						<div class="mr-4 rounded-full bg-emerald-100 p-3">
							<svg
								xmlns="http://www.w3.org/2000/svg"
								class="h-6 w-6 text-emerald-600"
								viewBox="0 0 20 20"
								fill="currentColor"
							>
								<path
									d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"
								/>
							</svg>
						</div>
						<div>
							<h4 class="text-lg font-medium text-gray-800">Academic Performance</h4>
							<p class="text-xl font-bold text-emerald-600">CGPA: 8.5</p>
						</div>
					</div>
				</div>
			</div>
		</section>

		<!-- Activities Section -->
		<section id="activities" class="bg-white py-20">
			<div class="container mx-auto px-4">
				<h2 class="relative mb-16 text-center text-3xl font-bold md:text-4xl">
					Extracurricular Activities
					<span class="mx-auto mt-4 block h-1 w-20 bg-emerald-600"></span>
				</h2>

				<div class="mx-auto grid max-w-5xl grid-cols-1 gap-8 md:grid-cols-2">
					{#each activities as activity, i}
						<div
							class="transform rounded-xl border-l-4 border-emerald-500 bg-white p-8 shadow-md transition-all duration-300 hover:scale-105 hover:shadow-xl"
							in:fly={{ x: i % 2 === 0 ? -30 : 30, duration: 500, delay: i * 150 }}
						>
							<div class="mb-4 flex items-start justify-between">
								<h3 class="text-xl font-bold text-emerald-600">{activity.title}</h3>
								<span
									class="rounded-full bg-emerald-100 px-3 py-1 text-sm font-medium text-emerald-800"
								>
									{activity.year}
								</span>
							</div>
							<p class="mb-4 font-medium text-gray-700">{activity.organization}</p>
							<ul class="space-y-3">
								{#each activity.description as point}
									<li class="flex items-start">
										<svg
											xmlns="http://www.w3.org/2000/svg"
											class="mt-0.5 mr-2 h-5 w-5 flex-shrink-0 text-emerald-500"
											viewBox="0 0 20 20"
											fill="currentColor"
										>
											<path
												fill-rule="evenodd"
												d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z"
												clip-rule="evenodd"
											/>
										</svg>
										<span class="text-gray-600">{point}</span>
									</li>
								{/each}
							</ul>
						</div>
					{/each}
				</div>
			</div>
		</section>

		<!-- Contact Section -->
		<section
			id="contact"
			class="relative overflow-hidden bg-gradient-to-br from-gray-50 to-emerald-50 py-20"
		>
			<!-- Background Pattern -->
			<div
				class="absolute inset-0 bg-[url('data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNjAiIGhlaWdodD0iNjAiIHZpZXdCb3g9IjAgMCA2MCA2MCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj48ZyBmaWxsPSJub25lIiBmaWxsLXJ1bGU9ImV2ZW5vZGQiPjxnIGZpbGw9IiMwMDk2ODgiIGZpbGwtb3BhY2l0eT0iMC4wMyI+PHBhdGggZD0iTTM2IDM0aDR2MWgtNHYtMXptMC0yaDF2NGgtMXYtNHptMi0yaDF2MWgtMXYtMXptLTIgMmgxdjFoLTF2LTF6bS0yLTJoMXYxaC0xdi0xem0yLTJoMXYxaC0xdi0xem0tMiAyaDF2MWgtMXYtMXptLTItMmgxdjFoLTF2LTF6Ii8+PC9nPjwvZz48L3N2Zz4=')] opacity-30"
			></div>

			<div class="relative z-10 container mx-auto px-4">
				<h2 class="relative mb-16 text-center text-3xl font-bold md:text-4xl">
					Get In Touch
					<span class="mx-auto mt-4 block h-1 w-20 bg-emerald-600"></span>
				</h2>

				<div class="mx-auto flex max-w-5xl flex-col gap-10 md:flex-row">
					<div class="md:w-1/2">
						<h3 class="mb-6 text-2xl font-semibold text-gray-800">Contact Information</h3>
						<p class="mb-8 text-lg text-gray-600">
							Feel free to reach out to me for any opportunities, collaborations, or just to say
							hello!
						</p>

						<div class="space-y-8">
							<div class="flex items-start">
								<div class="mr-4 rounded-full bg-emerald-100 p-3 shadow-md">
									<svg
										xmlns="http://www.w3.org/2000/svg"
										class="h-6 w-6 text-emerald-600"
										fill="none"
										viewBox="0 0 24 24"
										stroke="currentColor"
									>
										<path
											stroke-linecap="round"
											stroke-linejoin="round"
											stroke-width="2"
											d="M3 5a2 2 0 012-2h3.28a1 1 0 01.948.684l1.498 4.493a1 1 0 01-.502 1.21l-2.257 1.13a11.042 11.042 0 005.516 5.516l1.13-2.257a1 1 0 011.21-.502l4.493 1.498a1 1 0 01.684.949V19a2 2 0 01-2 2h-1C9.716 21 3 14.284 3 6V5z"
										/>
									</svg>
								</div>
								<div>
									<h4 class="text-lg font-medium text-gray-800">Phone</h4>
									<p class="text-gray-600">8310388742</p>
								</div>
							</div>

							<div class="flex items-start">
								<div class="mr-4 rounded-full bg-emerald-100 p-3 shadow-md">
									<svg
										xmlns="http://www.w3.org/2000/svg"
										class="h-6 w-6 text-emerald-600"
										fill="none"
										viewBox="0 0 24 24"
										stroke="currentColor"
									>
										<path
											stroke-linecap="round"
											stroke-linejoin="round"
											stroke-width="2"
											d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"
										/>
									</svg>
								</div>
								<div>
									<h4 class="text-lg font-medium text-gray-800">Email</h4>
									<p class="text-gray-600">amaresham50@gmail.com</p>
								</div>
							</div>

							<div class="flex items-start">
								<div class="mr-4 rounded-full bg-emerald-100 p-3 shadow-md">
									<svg
										xmlns="http://www.w3.org/2000/svg"
										class="h-6 w-6 text-emerald-600"
										fill="none"
										viewBox="0 0 24 24"
										stroke="currentColor"
									>
										<path
											stroke-linecap="round"
											stroke-linejoin="round"
											stroke-width="2"
											d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"
										/>
										<path
											stroke-linecap="round"
											stroke-linejoin="round"
											stroke-width="2"
											d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"
										/>
									</svg>
								</div>
								<div>
									<h4 class="text-lg font-medium text-gray-800">Location</h4>
									<p class="text-gray-600">Karnataka, Raichur, Uppala</p>
								</div>
							</div>

							<div class="flex items-start">
								<div class="mr-4 rounded-full bg-emerald-100 p-3 shadow-md">
									<svg
										xmlns="http://www.w3.org/2000/svg"
										class="h-6 w-6 text-emerald-600"
										fill="none"
										viewBox="0 0 24 24"
										stroke="currentColor"
									>
										<path
											stroke-linecap="round"
											stroke-linejoin="round"
											stroke-width="2"
											d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14"
										/>
									</svg>
								</div>
								<div>
									<h4 class="text-lg font-medium text-gray-800">Social</h4>
									<div class="mt-3 flex space-x-4">
										<a
											href="https://linkedin.com/in/amaresha-m-44b868249"
											target="_blank"
											class="transform rounded-full bg-white p-2 text-gray-600 shadow-md transition-colors duration-300 hover:scale-110 hover:text-emerald-600"
										>
											<svg
												xmlns="http://www.w3.org/2000/svg"
												class="h-6 w-6"
												fill="currentColor"
												viewBox="0 0 24 24"
											>
												<path
													d="M19 0h-14c-2.761 0-5 2.239-5 5v14c0 2.761 2.239 5 5 5h14c2.762 0 5-2.239 5-5v-14c0-2.761-2.238-5-5-5zm-11 19h-3v-11h3v11zm-1.5-12.268c-.966 0-1.75-.79-1.75-1.764s.784-1.764 1.75-1.764 1.75.79 1.75 1.764-.783 1.764-1.75 1.764zm13.5 12.268h-3v-5.604c0-3.368-4-3.113-4 0v5.604h-3v-11h3v1.765c1.396-2.586 7-2.777 7 2.476v6.759z"
												/>
											</svg>
										</a>
										<a
											href="https://github.com/Amaresha07"
											target="_blank"
											class="transform rounded-full bg-white p-2 text-gray-600 shadow-md transition-colors duration-300 hover:scale-110 hover:text-emerald-600"
										>
											<svg
												xmlns="http://www.w3.org/2000/svg"
												class="h-6 w-6"
												fill="currentColor"
												viewBox="0 0 24 24"
											>
												<path
													d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"
												/>
											</svg>
										</a>
									</div>
								</div>
							</div>
						</div>
					</div>

					<div class="md:w-1/2">
						<div
							class="transform rounded-xl bg-white p-8 shadow-lg transition-transform duration-300 hover:scale-105"
						>
							<h3 class="mb-6 text-2xl font-semibold text-gray-800">Send Me a Message</h3>

							{#if formSubmitted}
								<div class="mb-6 rounded-lg bg-emerald-100 p-4 text-emerald-700" transition:fade>
									Thank you for your message! I'll get back to you soon.
								</div>
							{/if}

							<form on:submit|preventDefault={handleSubmit} class="space-y-6">
								<div>
									<label for="name" class="mb-1 block text-sm font-medium text-gray-700">Name</label
									>
									<input
										type="text"
										id="name"
										bind:value={name}
										required
										class="w-full rounded-lg border border-gray-300 px-4 py-3 transition-colors duration-300 focus:border-emerald-500 focus:ring-2 focus:ring-emerald-500"
										placeholder="Your name"
									/>
								</div>

								<div>
									<label for="email" class="mb-1 block text-sm font-medium text-gray-700"
										>Email</label
									>
									<input
										type="email"
										id="email"
										bind:value={email}
										required
										class="w-full rounded-lg border border-gray-300 px-4 py-3 transition-colors duration-300 focus:border-emerald-500 focus:ring-2 focus:ring-emerald-500"
										placeholder="your.email@example.com"
									/>
								</div>

								<div>
									<label for="message" class="mb-1 block text-sm font-medium text-gray-700"
										>Message</label
									>
									<textarea
										id="message"
										bind:value={message}
										required
										rows="4"
										class="w-full rounded-lg border border-gray-300 px-4 py-3 transition-colors duration-300 focus:border-emerald-500 focus:ring-2 focus:ring-emerald-500"
										placeholder="Your message here..."
									></textarea>
								</div>

								<button
									type="submit"
									class="flex w-full transform items-center justify-center rounded-lg bg-emerald-600 px-6 py-3 font-medium text-white transition-all duration-300 hover:scale-105 hover:bg-emerald-700 hover:shadow-lg"
								>
									<svg
										xmlns="http://www.w3.org/2000/svg"
										class="mr-2 h-5 w-5"
										fill="none"
										viewBox="0 0 24 24"
										stroke="currentColor"
									>
										<path
											stroke-linecap="round"
											stroke-linejoin="round"
											stroke-width="2"
											d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"
										/>
									</svg>
									Send Message
								</button>
							</form>
						</div>
					</div>
				</div>
			</div>
		</section>
	</main>

	<!-- Footer -->
	<footer class="bg-gray-900 py-10 text-white">
		<div class="container mx-auto px-4">
			<div class="flex flex-col items-center justify-between md:flex-row">
				<div class="mb-6 md:mb-0">
					<a href="#home" class="flex items-center text-2xl font-bold text-emerald-400">
						<span
							class="mr-2 flex h-10 w-10 items-center justify-center rounded-full bg-emerald-400 text-gray-900"
							>AM</span
						>
						<span>Amaresha M</span>
					</a>
					<p class="mt-2 text-gray-400">Electronics & Communication Engineer</p>
				</div>

				<div class="flex flex-wrap justify-center gap-4 md:gap-6">
					{#each navLinks as link}
						<a
							href={link.href}
							class="text-gray-400 transition-colors duration-300 hover:text-emerald-400"
							on:click|preventDefault={() => scrollToSection(link.href.substring(1))}
						>
							{link.name}
						</a>
					{/each}
				</div>
			</div>

			<div class="mt-8 border-t border-gray-800 pt-8 text-center text-gray-400">
				<p>&copy; {new Date().getFullYear()} Amaresha M. All rights reserved.</p>
			</div>
		</div>
	</footer>
</div>

<style>
	:global(html) {
		scroll-behavior: smooth;
	}

	:global(body) {
		font-family:
			'Inter',
			-apple-system,
			BlinkMacSystemFont,
			'Segoe UI',
			Roboto,
			Oxygen,
			Ubuntu,
			Cantarell,
			'Open Sans',
			'Helvetica Neue',
			sans-serif;
	}

	/* Animation classes */
	@keyframes blob {
		0% {
			transform: translate(0px, 0px) scale(1);
		}
		33% {
			transform: translate(30px, -50px) scale(1.1);
		}
		66% {
			transform: translate(-20px, 20px) scale(0.9);
		}
		100% {
			transform: translate(0px, 0px) scale(1);
		}
	}

	.animate-blob {
		animation: blob 7s infinite;
	}

	.animation-delay-2000 {
		animation-delay: 2s;
	}

	.animation-delay-4000 {
		animation-delay: 4s;
	}
</style>
