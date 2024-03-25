<script lang="ts">
  import { ethers } from "ethers";
  import type { JsonRpcSigner } from "ethers";
  import { Contract } from "ethers";
  import { ABI } from "./abi";

  let balance: number = 0;
  let staked: number = 0;

  const connectWallet = async () => {
    const { ethereum } = window as any;
    const provider = new ethers.BrowserProvider(ethereum);
    const account = await provider.send("eth_accounts", []);
    console.log(account);

    const signer = await provider.getSigner();
    const contract = await initializeContract(signer);
    balance = await contract.balanceOf(signer)
    staked = await contract.getStake(signer)
  };

  const mint = async () => {
    const { ethereum } = window as any;
    const provider = new ethers.BrowserProvider(ethereum);
    const signer = await provider.getSigner();
    const contract = await initializeContract(signer);

    const toMintInput = document.getElementById('toMint') as HTMLInputElement;
    const toMintValue = parseInt(toMintInput.value);

    await contract.mint(signer, toMintValue);
    alert("You know how it feels to mint 5 gum")
    balance = await contract.balanceOf(signer)
    staked = await contract.getStake(signer)
}

const stake = async () => {
    const { ethereum } = window as any;
    const provider = new ethers.BrowserProvider(ethereum);
    const signer = await provider.getSigner();
    const contract = await initializeContract(signer);

    const toStakeInput = document.getElementById('toStake') as HTMLInputElement;
    const toStakeValue = parseInt(toStakeInput.value);

    await contract.stake(toStakeValue);
    alert("Welcome to the Stake F1 team")
    balance = await contract.balanceOf(signer)
    staked = await contract.getStake(signer)
}

  const withdraw = async () => {
    const { ethereum } = window as any;
    const provider = new ethers.BrowserProvider(ethereum);
    const signer = await provider.getSigner();
    const contract = await initializeContract(signer);

    await contract.withdraw();
    alert("Stonks!")
    balance = await contract.balanceOf(signer)
    staked = await contract.getStake(signer)
  }

  const initializeContract = async (signer: JsonRpcSigner) => {
    return new Contract(
      "0xEc581f1Ef6e8688F328342d00C52a5765D4Cd0FA",
      ABI,
      signer
    );
  };
</script>

<main>
  <h1 style="color:Orange;background-color:Black">FakeCoin</h1>
  <h2>You give {balance} FAKCs</h2>
  <h2>You have {staked} stakes in Stake F1 team</h2>
  <p><input type="number" id="toMint" placeholder="Mint Amount"></p>
  <p><input type="number" id="toStake" placeholder="Stake Amount"></p>
  <button on:click={connectWallet}>Connect Wallet/Refresh Data</button>
  <button on:click={stake}>Stake F1 team</button>
  <button on:click={mint}>Mint here to find out how it feels to chew 5 gum</button>
  <button on:click={withdraw}>Withdraw and Get Stonks</button>
</main>