<script lang="ts">
	import Fa from 'svelte-fa';
	import { faGithub, faLinkedin } from '@fortawesome/free-brands-svg-icons';
	import { faGlobe } from '@fortawesome/free-solid-svg-icons';
</script>

<div
	class="col-span-1 row-span-3 flex flex-col items-center justify-evenly rounded-xl bg-accent/80 shadow-md md:row-span-4 lg:row-span-5"
>
	<a href="https://github.com/adhyaksasb" class="flex items-center justify-center"
		><Fa icon={faGithub} class="text-2xl hover:scale-110 md:text-5xl" /></a
	>
	<a href="https://www.linkedin.com/in/adhyaksasb/" class="flex items-center justify-center"
		><Fa icon={faLinkedin} class="text-2xl hover:scale-110 md:text-5xl" /></a
	>
	<a href="https://adhyaksasb.netlify.app/" class="flex items-center justify-center"
		><Fa icon={faGlobe} class="text-2xl hover:scale-110 md:text-5xl" /></a
	>
</div>
