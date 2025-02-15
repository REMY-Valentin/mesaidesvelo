<script>
	import MiniatureCollectivite from '$lib/components/MiniatureCollectivite.svelte';
	import Search from '$lib/components/Search.svelte';

	/** @type {import('./$types').PageData}*/
	export let data;
</script>

<svelte:head>
	<title>Les aides au vélo dans {data.departement.determinant}{data.departement.nom}</title>
	<meta
		name="description"
		content="Découvrez l’ensemble des aides au vélo proposées dans {data.departement
			.determinant}{data.departement.nom}. Simple, rapide et gratuit."
	/>
</svelte:head>

<div class="prose w-full max-w-screen-sm m-auto mt-8">
	<h1>Les aides à l’achat de vélo dans {data.departement.determinant}{data.departement.nom}</h1>

	<p>
		Vous souhaitez acheter un vélo et vous habitez dans {data.departement.determinant}{data
			.departement.nom} ? Découvrez les subventions auxquelles vous avez droit !
	</p>

	{#if !data.aideDepartement && !data.aideRegion}
		<h2>Aide départementale et régionale</h2>
		<p>
			La première chose à noter est que ni le département {data.departement.nom}, ni la région {data
				.region.nom}
			ne proposent de subvention pour l’achat d’un vélo.
		</p>
	{:else}
		<h2>Au niveau du département</h2>
		{#if data.aideDepartement}
			{#if data.aideDepartementText}
				<MiniatureCollectivite ruleName={data.aideDepartement} titre={data.departement.nom} />
				{@html data.aideDepartementText}
			{:else}
				<p>Il n'y a une aide au niveau du département.</p>
			{/if}
		{:else}
			<p>
				La première chose à noter est que le département {data.departement.nom} ne propose pas d’aide
				à l’achat de vélo.
			</p>
			<p>
				Cela est relativement normal car même si certains departements proposent une aide,
				l’organisation des mobilités relève normalement des compétences des régions ou des
				intercommunes et métropoles.
			</p>
			<p>
				Heureusement les {data.departement.habitants} peuvent bénéficier de l’aide de la région.
			</p>
		{/if}
		{#if data.aideRegion}
			{#if data.aideRegionText}
				<h2>Au niveau de la région {data.region.nom}</h2>
				<MiniatureCollectivite ruleName={data.aideRegion} titre={data.region.nom} />
				{@html data.aideRegionText}
			{:else}
				<p>En revanche la région {data.region.nom} propose une aide à l’achat de vélo.</p>
			{/if}
		{:else}
			<p>En revanche la région {data.region.nom} ne propose pas d’aide à l’achat de vélo.</p>
		{/if}
	{/if}

	<h2>Les aides locales</h2>
	<p>
		Pour simuler le montant exact des aides auxquelles vous avez droit en cumulant les aides
		proposées par le département, la région, et l’Etat, nous avons développé un calculateur.
		Renseignez simplement votre ville de résidence ci-dessous :
	</p>

	<Search />

	{#if data.aidesLocales.length > 0}
		<p>Dans le département les collectivités suivantes proposent des aides supplémentaires :</p>
		<ul>
			{#each data.aidesLocales as { titre, slug }}
				<li><a href="/ville/{slug}">{titre}</a></li>
			{/each}
		</ul>
	{/if}
	<h2>Les aides de l’État</h2>
	<p>
		Et bien sûr les {data.departement.habitants} peuvent bénéficier des aides de l’Etat. Il s’agit :
	</p>
	<ul>
		<li>Du bonus écologique, ouvert sous conditions à certains type de vélo</li>
		<li>
			De la <a href="/prime-a-la-conversion">prime à la conversion</a>, conditionnée à la remise à
			la casse d’une voiture ancienne
		</li>
		<li>
			Du <a href="/forfait-mobilite-durable">forfait mobilités durables</a>, versé par l’employeur
			pour prendre en charge les frais liés aux déplacements domicile-travail réalisés à vélo
		</li>
	</ul>
</div>
