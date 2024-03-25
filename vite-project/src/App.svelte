<script lang="ts">
  import { ethers } from "ethers";
  import type { JsonRpcSigner } from "ethers";
  import { Contract } from "ethers";
  import { ABI } from "./abi";

  let data: number = 0;
  const connectWallet = async () => {
    const { ethereum } = window as any;
    const provider = new ethers.BrowserProvider(ethereum);
    const account = await provider.send("eth_accounts", []);
    console.log(account);
  };

  const initializeContract = async (signer: JsonRpcSigner) => {
    return new Contract(
      "0xEc581f1Ef6e8688F328342d00C52a5765D4Cd0FA",
      ABI,
      signer
    );
  };
</script>

<!--  

  [√] 1. Connect wallet
  2. Read Functions 
  3. Write Functions
-->

<main>
  <button on:click={connectWallet}>Connect Wallet</button>
</main>