<script lang="ts">
	import Fa from 'svelte-fa';
	import { faFlag, faUpRightFromSquare } from '@fortawesome/free-solid-svg-icons';
	export let url: string;
	export let text: string;
</script>

<a class="link-secondary m-0 inline-flex" href={url} target="_blank"
	>{text} <Fa class="pl-1" icon={faUpRightFromSquare} size="xs" />
</a>
