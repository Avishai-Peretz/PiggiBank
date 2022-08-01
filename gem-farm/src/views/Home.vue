<meta name="viewport" content="width=1000; user-scalable=0;" />
<style>
body {
/* Location of the image */
/* background-image: url("BankBG.png"); */
/* Background image is centered vertically and horizontally at all times */
background-position: center center;

/* Background image doesn't tile */
background-repeat: no-repeat;

/* Background image is fixed in the viewport so that it doesn't move when
the content's height is greater than the image's height */
background-attachment: fixed;

/* This is what makes the background image rescale based
on the container's size */
background-size: cover;

/* Set a background color that will be displayed
while the background image is loading */
background-color: #3b004b;
width: 100vw !important;
}
.center{
  grid-area: center;
  background-position: center center;
  background-repeat: no-repeat;
  background-attachment: fixed;
  background-size: cover;
  background-color: #cca374 !important;
  border: none !important;
  margin: 0 !important;
}
.container-1 {  
  margin-top: 10px;
  display: grid;
  width: 90vw;
  grid-template-columns: 30px 2.9fr 30px;
  grid-template-rows: 30px 2fr 30px;
  gap: 0px 0px;
  grid-auto-flow: row;
  grid-template-areas:
    "l-t top r-t"
    "l-c center r-c"
    "l-b bottom r-b";
}
.container-2-3-4{
  display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
  .container-2 {  
    margin-top: 10px;
    align-self: center;
    width: 90vw;
    display: grid;
    grid-template-columns: 30px 2.9fr 30px;
    grid-template-rows: 30px 2.5fr 30px;
    gap: 0px 0px;
    grid-auto-flow: row;
    grid-template-areas:
      "l-t top r-t"
      "l-c center r-c"
      "l-b bottom r-b";
  }
  .container-3, .container-4 {  
    align-self: center;
    width: 40%;
    display: grid;
     grid-template-columns: 30px 2.7fr 30px;
    grid-template-rows: 30px 2.4fr 30px;
    gap: 0px 0px;
    grid-auto-flow: row;
    grid-template-areas:
      "l-t top r-t"
      "l-c center r-c"
      "l-b bottom r-b";
  }
  .body-container{
    margin: 0;
    width: 90vw;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  .footer {
  justify-content: center;
  }
  .p-10{
    padding: 0 !important;
  }

.r-c { 
  grid-area: r-c;
  background: url(./boxes/rightmid.png) center center/contain;
  }

.l-c {
   grid-area: l-c; 
background: url(./boxes/leftmid.png) center center/contain;
}

.top {
   grid-area: top; 
   background: url(./boxes/topmid.png) center center/contain;
}

.l-t {
   grid-area: l-t;
   background: url(./boxes/topleft.png) center center/cover no-repeat;
}

.bottom {
   grid-area: bottom; 
   background: url(./boxes/bottommid.png) center center/contain;

}

.l-b { 
  grid-area: l-b; 
background: url(./boxes/bottomleft.png) center center/cover no-repeat;
}

.r-b {
   grid-area: r-b;
 background: url(./boxes/bottomright.png) center center/cover no-repeat;
 }

.r-t { 
  grid-area: r-t; 
background: url(./boxes/topright.png) center center/cover no-repeat;
}

</style>
<template >
  <ConfigPane />
  <div v-if="!wallet" class="text-center">Please connect your wallet to continue</div>
  <div class="body-container" v-else>
    <!--farm address-->
    <div class="container-1">
      <div class="r-c"></div>
      <div class="l-c"></div>
      <div class="top"></div>
      <div class="l-t"></div>
      <div class="bottom"></div>
      <div class="l-b"></div>
      <div class="r-b"></div>
      <div class="r-t"></div>      
      <div class="nes-container mb-10 center">
        <p class="nes-text is-warning">Connect to our Piggybank 🐷💰</p>
        <BR></BR>
        <div class="nes-field mb-5">
          <label for="farm">Please select corresponding Project Vault</label>
          <div class="nes-select is-warning">
          <select required id="pet-select" v-model="farm">
          <option value="Ej51Lf2ZzipUvJz5VHezrqTW95DsDG1gMefhdi7Z2Thr">PlutoPigs BETA Vault</option>
          </select>
          </div>
        </div>
      </div>
    </div>
    <div v-if="farmerAcc" class="container-2-3-4">
    <div class="container-2">
      <div class="r-c"></div>
      <div class="l-c"></div>
      <div class="top"></div>
      <div class="l-t"></div>
      <div class="bottom"></div>
      <div class="l-b"></div>
      <div class="r-b"></div>
      <div class="r-t"></div>
       <FarmerDisplay
        :key="farmerAcc"
        :farm="farm"
        :farmAcc="farmAcc"
        :farmer="farmer"
        :farmerAcc="farmerAcc"
        class="mb-10 center"
        @refresh-farmer="handleRefreshFarmer"
      />
    </div>
      <Vault
        :key="farmerAcc"
        class="mb-10 center"
        :vault="farmerAcc.vault.toBase58()"
        @selected-wallet-nft="handleNewSelectedNFT"
      >
        <button
          v-if="farmerState === 'staked' && selectedNFTs.length > 0"
          class="nes-btn is-primary mr-5"
          @click="addGems"
        >
          Add Gems (resets staking)
        </button>
        <button
          v-if="farmerState === 'unstaked'"
          class="nes-btn is-success mr-5"
          @click="beginStaking"
        >
          Begin staking
        </button>
        <button
          v-if="farmerState === 'staked'"
          class="nes-btn is-error mr-5"
          @click="endStaking"
        >
          End staking
        </button>
        <button
          v-if="farmerState === 'pendingCooldown'"
          class="nes-btn is-error mr-5"
          @click="endStaking"
        >
          End cooldown
        </button>
        <button class="nes-btn is-warning" @click="claim">
          Claim {{ availableA / 1000000000 }} $BACON
        </button>
      </Vault>
    </div>
    <div v-else>
      <div class="w-full text-center text-white mb-5">
        Staker account not found :( Create a new one?
      </div>
      <div class="w-full text-center">
        <button class="nes-btn is-primary" @click="initFarmer">
          New Staker
        </button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, nextTick, onMounted, ref, watch } from 'vue';
import useWallet from '@/composables/wallet';
import useCluster from '@/composables/cluster';
import { initGemFarm } from '@/common/gem-farm';
import { PublicKey } from '@solana/web3.js';
import ConfigPane from '@/components/ConfigPane.vue';
import FarmerDisplay from '@/components/gem-farm/FarmerDisplay.vue';
import Vault from '@/components/gem-bank/Vault.vue';
import { INFT } from '@/common/web3/NFTget';
import { findFarmerPDA, stringifyPKsAndBNs } from '@gemworks/gem-farm-ts';
import numeral from 'numeral';
import { numberLiteralTypeAnnotation } from '@babel/types';

export default defineComponent({
  components: { Vault, FarmerDisplay, ConfigPane },
  setup() {
    const { wallet, getWallet } = useWallet();
    const { cluster, getConnection } = useCluster();

    let gf: any;
    watch([wallet, cluster], async () => {
      await freshStart();
    });

    //needed in case we switch in from another window
    onMounted(async () => {
      await freshStart();
    });

    // --------------------------------------- farmer details
    const farm = ref<string>();
    const farmAcc = ref<any>();

    const farmerIdentity = ref<string>();
    const farmerAcc = ref<any>();
    const farmerState = ref<string>();

    const availableA = ref<any>();
    const availableB = ref<string>();

    //auto loading for when farm changes
    watch(farm, async () => {
      await freshStart();
    });

    const updateAvailableRewards = async () => {
      availableA.value = farmerAcc.value.rewardA.accruedReward
        .sub(farmerAcc.value.rewardA.paidOutReward)
        .toNumber()
        .toFixed(9);
      availableB.value = farmerAcc.value.rewardB.accruedReward
        .sub(farmerAcc.value.rewardB.paidOutReward)
        .toString();
    };

    const fetchFarn = async () => {
      farmAcc.value = await gf.fetchFarmAcc(new PublicKey(farm.value!));
      console.log(
        `farm found at ${farm.value}:`,
        stringifyPKsAndBNs(farmAcc.value)
      );
    };

    const fetchFarmer = async () => {
      const [farmerPDA] = await findFarmerPDA(
        new PublicKey(farm.value!),
        getWallet()!.publicKey!
      );
      farmerIdentity.value = getWallet()!.publicKey?.toBase58();
      farmerAcc.value = await gf.fetchFarmerAcc(farmerPDA);
      farmerState.value = gf.parseFarmerState(farmerAcc.value);
      await updateAvailableRewards();
      console.log(
        `farmer found at ${farmerIdentity.value}:`,
        stringifyPKsAndBNs(farmerAcc.value)
      );
    };

    const freshStart = async () => {
      if (getWallet() && getConnection()) {
        gf = await initGemFarm(getConnection(), getWallet()!);
        farmerIdentity.value = getWallet()!.publicKey?.toBase58();

        //reset stuff
        farmAcc.value = undefined;
        farmerAcc.value = undefined;
        farmerState.value = undefined;
        availableA.value = undefined;
        availableB.value = undefined;

        try {
          await fetchFarn();
          await fetchFarmer();
        } catch (e) {
          console.log(`farm with PK ${farm.value} not found :(`);
        }
      }
    };

    const initFarmer = async () => {
      await gf.initFarmerWallet(new PublicKey(farm.value!));
      await fetchFarmer();
    };

    // --------------------------------------- staking
    const beginStaking = async () => {
      await gf.stakeWallet(new PublicKey(farm.value!));
      await fetchFarmer();
      selectedNFTs.value = [];
    };

    const endStaking = async () => {
      await gf.unstakeWallet(new PublicKey(farm.value!));
      await fetchFarmer();
      selectedNFTs.value = [];
    };

    const claim = async () => {
      await gf.claimWallet(
        new PublicKey(farm.value!),
        new PublicKey(farmAcc.value.rewardA.rewardMint!),
        new PublicKey(farmAcc.value.rewardB.rewardMint!)
      );
      await fetchFarmer();
    };

    const handleRefreshFarmer = async () => {
      await fetchFarmer();
    };

    // --------------------------------------- adding extra gem
    const selectedNFTs = ref<INFT[]>([]);

    const handleNewSelectedNFT = (newSelectedNFTs: INFT[]) => {
      console.log(`selected ${newSelectedNFTs.length} NFTs`);
      selectedNFTs.value = newSelectedNFTs;
    };

    const addSingleGem = async (
      gemMint: PublicKey,
      gemSource: PublicKey,
      creator: PublicKey
    ) => {
      await gf.flashDepositWallet(
        new PublicKey(farm.value!),
        '1',
        gemMint,
        gemSource,
        creator
      );
      await fetchFarmer();
    };

    const addGems = async () => {
      await Promise.all(
        selectedNFTs.value.map((nft) => {
          const creator = new PublicKey(
            //todo currently simply taking the 1st creator
            (nft.onchainMetadata as any).data.creators[0].address
          );
          console.log('creator is', creator.toBase58());

          addSingleGem(nft.mint, nft.pubkey!, creator);
        })
      );
      console.log(
        `added another ${selectedNFTs.value.length} gems into staking vault`
      );
    };

    return {
      wallet,
      farm,
      farmAcc,
      farmer: farmerIdentity,
      farmerAcc,
      farmerState,
      availableA,
      availableB,
      initFarmer,
      beginStaking,
      endStaking,
      claim,
      handleRefreshFarmer,
      selectedNFTs,
      handleNewSelectedNFT,
      addGems,
    };
  },
});
</script>

<style scoped></style>
