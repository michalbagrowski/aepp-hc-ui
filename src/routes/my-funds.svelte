<script>
	import { getAddrShares, totalStakedForAddr } from '$lib/aesdk/contractState';

	import CoinAmount from '$lib/components/CoinAmount.svelte';
	import NavBarLink from '$lib/components/NavBarLink.svelte';
	import StakingCard from '$lib/components/StakingCard.svelte';
	import { validatorsStore } from '$lib/stores/validatorsSore';
	import { walletConnectionStore } from '$lib/stores/walletConnectionStore';
	import { state } from '@aeternity/aepp-sdk/es/channel/internal';
	import Validators from './validators.svelte';
	$: wallet = $walletConnectionStore.connectedWallet;
	$: vs = $validatorsStore;
	$: myValidators = vs
		? vs.validators.filter((v) => (wallet ? getAddrShares(v, wallet.addr) : false))
		: [];
	$: myTotalStake = wallet && vs ? totalStakedForAddr(vs, wallet.addr) : 0n;
</script>

<div class="container content-center border items-center">
	<div class="card border m-2 p-2 rounded  shadow max-w-3xl ">
		<div class="card-title"><h2 class="mt-4">My Funds</h2></div>
		<div class="card-body p-3">
			{#if wallet}
				<div class="alert bg-secondary/10 border border-secondary">
					Available in wallet: <CoinAmount aetto={wallet.balAETTO} dropdownReverse />
				</div>
				<div class="space-y-2">
					{#if vs}
						<div class="alert bg-primary/10 border border-primary/50">
							Total staked: <CoinAmount aetto={myTotalStake} dropdownReverse />
						</div>
						{#if !myValidators?.length}
							<div>
								<a sveltekit:prefetch href="/validators">
									<button class="btn btn-secondary rounded-xl"
										>Choose a validator to delegate</button
									>
								</a>
							</div>
						{/if}
						{#each myValidators as val (val.ct)}
							<StakingCard validator={val} />
						{/each}
					{:else}
						Fetching validators state...
					{/if}
				</div>
			{:else}
				<div class="alert alert-warning">Not connected to a wallet</div>
			{/if}
		</div>
	</div>
</div>
