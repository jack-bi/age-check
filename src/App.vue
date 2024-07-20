<template>
	<PreAgeCheck />
	<HomePage />

</template>

<script>
import HomePage from './components/HomePage.vue'
import PreAgeCheck from '@/components/PreAgeCheck'

export default {
	name: 'App',
	components: {
		HomePage,
		PreAgeCheck,
	}
}
</script>
