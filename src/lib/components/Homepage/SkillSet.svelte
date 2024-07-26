<script lang="ts">
	import Fa from 'svelte-fa';
	import {
		faHtml5,
		faJs,
		faCss3,
		faJava,
		faPhp,
		faGolang,
		faDocker,
		faGitAlt,
		faReact,
		faVuejs,
		faLaravel,
		faAws
	} from '@fortawesome/free-brands-svg-icons';
</script>

<div
	class="col-span-7 min-h-10 overflow-hidden rounded-xl bg-muted/40 shadow-lg md:col-span-6 md:row-span-1 lg:col-span-7 lg:row-span-1"
>
	<div class="flex h-full items-center justify-center gap-3">
		<span class="mr-2 hidden text-sm text-muted-foreground md:block"
			>Frameworks / Languages / Programs</span
		>
		<span title="HTML">
			<Fa icon={faHtml5} class="text-md md:text-2xl" />
		</span>
		<span title="CSS">
			<Fa icon={faCss3} class="text-md md:text-2xl" />
		</span>
		<span title="Javascript">
			<Fa icon={faJs} class="text-md md:text-2xl" />
		</span>
		<span title="Java">
			<Fa icon={faJava} class="text-md md:text-2xl" />
		</span>
		<span title="PHP">
			<Fa icon={faPhp} class="text-md md:text-2xl" />
		</span>
		<span title="Golang">
			<Fa icon={faGolang} class="text-md md:text-2xl" />
		</span>
		<span title="Docker">
			<Fa icon={faDocker} class="text-md md:text-2xl" />
		</span>
		<span title="Git">
			<Fa icon={faGitAlt} class="text-md md:text-2xl" />
		</span>
		<span title="React">
			<Fa icon={faReact} class="text-md md:text-2xl" />
		</span>
		<span title="Vue">
			<Fa icon={faVuejs} class="text-md md:text-2xl" />
		</span>
		<span title="Laravel">
			<Fa icon={faLaravel} class="text-md md:text-2xl" />
		</span>
		<span title="AWS">
			<Fa icon={faAws} class="text-md md:text-2xl" />
		</span>
	</div>
</div>
