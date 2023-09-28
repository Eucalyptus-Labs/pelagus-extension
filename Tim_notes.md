## pelagus-extension/background/constants/networks.ts

https://github.com/PelagusWallet/pelagus-extension/blob/164291ac1a4994992253bd68e33b4fc5770c2bfc/background/constants/networks.ts#L104C1-L117C26

export const DEFAULT_QUAI_TESTNET = {
  name: "Colosseum",
  chainCode: 994,
  chainID: 9000,
  isCustom: false,
  chains: [
    {
      name: "Cyprus One",
      shard: "cyprus-1",
      rpc: "https://rpc.cyprus1.colosseum.quaiscan.io",
      blockExplorerUrl: "https://cyprus1.colosseum.quaiscan.io"
    },
    {
      name: "Cyprus Two",


## pelagus-extension/background/redux-slices/assets.ts

https://github.com/PelagusWallet/pelagus-extension/blob/164291ac1a4994992253bd68e33b4fc5770c2bfc/background/redux-slices/assets.ts#L568

  const fields: any = [
      formatNumber(transaction.network.chainID || 0, "chainId"),
      formatNumber(transaction.nonce || 0, "nonce"),
      formatNumber(transaction.maxPriorityFeePerGas || 0, "maxPriorityFeePerGas"),
      formatNumber(transaction.maxFeePerGas || 0, "maxFeePerGas"),
      formatNumber(transaction.gasLimit || 0, "gasLimit"),
      (transaction.to), // presuming it's valid as it's already signed
      formatNumber(transaction.value || 0, "value"),
      (transaction.input || "0x"),
      (formatAccessList([]))
  ];
      if (transaction.type == 2) {
        fields.push(formatNumber(transaction.externalGasLimit || 0, "externalGasLimit"))
        fields.push(formatNumber(transaction.externalGasPrice || 0, "externalGasPrice"))
        fields.push(formatNumber(transaction.externalGasTip || 0, "externalGasTip"))
        fields.push(/*transaction.externalData ||*/ "0x")
        fields.push(formatAccessList(/*transaction.externalAccessList ||*/ []))
        fields.push(formatNumber(transaction.v, "recoveryParam"));
        fields.push(stripZeros(transaction.r));


## shard from address algo (networks.ts)

export const getShardFromAddress = function(address: string): string {
  if (address === "") {
    console.log("Address is empty or zero")
    return "cyprus-1" // Technically zero address is in every shard, but we return a default instead
  }
  let shardData = QUAI_CONTEXTS.filter((obj) => {
    const num = Number(address.substring(0, 4))
    const start = Number("0x" + obj.byte[0])
    const end = Number("0x" + obj.byte[1])
    return num >= start && num <= end
  })
  if (shardData.length === 0) {
    throw new Error("Invalid address")
}
return shardData[0].shard
}

## Fee multiplier for external TXs (assets.ts)

}

// Emitted ETXs must include some multiple of BaseFee as miner tip, to
// encourage processing at the destination.
export function calcEtxFeeMultiplier(fromShard: string, toShard: string) {
	let multiplier = NUM_ZONES_IN_REGION
  if(fromShard == undefined || toShard == undefined || fromShard == toShard) {
    console.error("invalid shards in calcEtxFeeMultiplier: " + fromShard + " " + toShard)
    return 0 // will cause an error
  }
	if (fromShard.includes("cyprus") && toShard.includes("cyprus") || fromShard.includes("paxos") && toShard.includes("paxos") || fromShard.includes("hydra") && toShard.includes("hydra")) {
    // same region
		multiplier = NUM_ZONES_IN_REGION
	} else {
    multiplier = NUM_ZONES_IN_REGION * NUM_REGIONS_IN_PRIME
  }
	return multiplier
}

gasLimit??BigInt(200000), maxFeePerGas??BigInt(2000000000), maxPriorityFeePerGas??BigInt(1000000000))
