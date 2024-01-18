# Solana Swap Guide


## 获取报价
- Request
```
curl -s 'https://quote-api.jup.ag/v6/quote?inputMint=So11111111111111111111111111111111111111112&outputMint=EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v&amount=100000&slippageBps=50&onlyDirectRoutes=false&asLegacyTransaction=false&maxAccounts=64&experimentalDexes=Jupiter%20LO'
```
- Response
```
{"inputMint":"So11111111111111111111111111111111111111112","inAmount":"100000","outputMint":"EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v","outAmount":"9752","otherAmountThreshold":"9704","swapMode":"ExactIn","slippageBps":50,"platformFee":null,"priceImpactPct":"0","routePlan":[{"swapInfo":{"ammKey":"EkDQqNFijZHQDKfM8KcNBFb3UUGaJRuZpJkj1Zu2BYFN","label":"Whirlpool","inputMint":"So11111111111111111111111111111111111111112","outputMint":"mSoLzYCxHdYgdzU16g5QSh3i5K3z3KZK7ytfqcJm7So","inAmount":"100000","outAmount":"86164","feeAmount":"39","feeMint":"So11111111111111111111111111111111111111112"},"percent":100},{"swapInfo":{"ammKey":"5Qwbha7hhDHWcVLZn26A2etHE3v5Db4yDVJYPdD8c6wj","label":"GooseFX","inputMint":"mSoLzYCxHdYgdzU16g5QSh3i5K3z3KZK7ytfqcJm7So","outputMint":"EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v","inAmount":"86164","outAmount":"9752","feeAmount":"9","feeMint":"EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v"},"percent":100}],"contextSlot":242321550,"timeTaken":0.008142265}
```


## 提交报价信息获取Swap Info
- Request
```
curl -L 'https://quote-api.jup.ag/v6/swap' \
-H 'Content-Type: application/json' \
-H 'Accept: application/json' \
-d '{"userPublicKey":"4CiHvYgc4FehCHs9PhRt4Ua7V54EPmADJFt5tSYCvdqL","wrapAndUnwrapSol":true,"useSharedAccounts":true,"prioritizationFeeLamports":"auto","asLegacyTransaction":true,"dynamicComputeUnitLimit":true,"quoteResponse":{"inputMint":"So11111111111111111111111111111111111111112","inAmount":"10000000","outputMint":"EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v","outAmount":"976604","otherAmountThreshold":"971721","swapMode":"ExactIn","slippageBps":50,"priceImpactPct":"0","routePlan":[{"swapInfo":{"ammKey":"4DoNfFBfF7UokCC2FQzriy7yHK6DY6NVdYpuekQ5pRgg","label":"Phoenix","inputMint":"So11111111111111111111111111111111111111112","outputMint":"EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v","inAmount":"10000000","outAmount":"976604","feeAmount":"196","feeMint":"EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v"},"percent":100}],"contextSlot":242128063,"timeTaken":0.011169626}}'
```
- Response
```
{"swapTransaction":"xxxxxxxxxx","lastValidBlockHeight":222996397,"prioritizationFeeLamports":14000}
```

## 广播交易
- Request
```
curl https://api.mainnet-beta.solana.com/ \
-X POST \
-H "Content-Type: application/json" \
--data '{"method":"sendTransaction","params":["AbUluPVIvLO+wK1gXLnmcIujpLYuHp6ezzEwAtRj+YF88YfIg3Uf+nAkKisjchUllrch2kMnsJzNsa4IdoZzuQ8BAAsWL5MjcP0ElSKS7nWnvOk+qZvJ6jITGFotkuy1gKFqIhtdOId10QLainQ5kzwHfMaC4rvzdhQVneJ+opX/GIY+cmfoQmV1vuOnlEteOjdhK8N5fGORvWjRVZoPfrFOaF/DaMEMOsetxy4/X7Kmhyoce4NRtLjDpZDBSFFBP+GNhGZrecov+9zIz+cfV1JS4rOaDTTnvXB2gZYoTf65yfavCHcEejgcORU496O6Qrr+hB1FPybVLnGmZEP2rx7ddIr9kvVuIilw1wUBEUeK6PtY2OWjPoJse3/Di4U1Y81rc++y3BV9Qu6EMXdWDvCGUaRoBFCNbceAkSwBiK5UwJyYS8KJvs9JKhIgu2tUPBQPmfBf4T8KqJiMkXpLfXYIpdJz6dRIiwf+OZsakVXlghtpfUMBbAo8Tzu8oq+0HQFjMFcJdwwmHhSrigVrzDJPZhJygIhZeQzeBEs8T3hiy8joHQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAjJclj04kifG7PRApFI4NgwtaE5na/xCEBI572Nvp+FmsGuPQh/KSNwYlSPcMTASuwqmVaUmG58u0Z1IGIdOGMAMGRm/lIRcy/+ytunLDm+e8jOW7xfcSayxDmzpAAAAAtD/6J/XX9kp0wJsfKVh53ksJqzbfyd1RSzIap7OM5ejG+nrzvtutOj1l82qryXQxsbvkwtL24OR8pgIDRS9dYezQTboeJIGdp9CZ10s1k9rnH0Vw5uDAWYp94Gl1fvFrBHnVW/IxwG7udMVuzmgVB/2xst6j9I5RArHNola8E48Gm4hX/quBhPtof2NGGMA12sQ53BrrO1WYoPAAAAAAAQbd9uHXZaGT2cvhRs7reawctIXtX1s3kTqM9YV+/wCpDgNoX46QkFPkWBIcZvWnau3HcGqhHIL4qpUqjyt4eal6Xol4k88mWEZKgI1+HcOJYJoStrZTk3oEdBHH4jJ/LgUOAAUCt9cCAA4ACQMQJwAAAAAAAAwGAAEAEwsUAQESGRQNAAgJBQEQExISDxIVFA0DBQcJCgYCBBElwSCbM0HWnIEFAQAAABEAZAABOxEPAAAAAACMLpoAAAAAADIAABQDAQAAAQk=",{"encoding":"base64"}],"id":1,"jsonrpc":"2.0"}'

```
- Response
```
{"jsonrpc":"2.0","result":"2ejbCbH1BB2mNb4jtHc5UrfKSsLSED9MRh4ZWuNn2x5zEHEWKA2Ft6KfkdXzfyQ4VDwDPvPTwnLm7Lg53SMfLyh8","id":1}
```


## 获取SOL余额
- Request
```
curl https://api.mainnet-beta.solana.com/ \
-X POST \
-H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0", "id":1, "method":"getBalance", "params":["4CiHvYgc4FehCHs9PhRt4Ua7V54EPmADJFt5tSYCvdqL"]}'

```
- Response
```
{"jsonrpc":"2.0","result":{"context":{"apiVersion":"1.16.23","slot":242281868},"value":70668000094},"id":1}
```


## 获取SPL Token余额
```
curl https://api.mainnet-beta.solana.com/  \
-X POST \
-H "Content-Type: application/json" \
--data '{"jsonrpc": "2.0","id": 1,"method": "getTokenAccountsByOwner","params": ["4CiHvYgc4FehCHs9PhRt4Ua7V54EPmADJFt5tSYCvdqL",{"mint": "BoZoQQRAmYkr5iJhqo7DChAs7DPDwEZ5cv1vkYC9yzJG"},{"encoding": "jsonParsed"}]}'
```



## 获取交易状态，如果result返回null则交易还未确认
```
curl https://docs-demo.solana-mainnet.quiknode.pro/ \
-X POST \
-H "Content-Type: application/json" \
--data '{"jsonrpc": "2.0","id": 1,"method": "getTransaction","params": ["2ejbCbH1BB2mNb4jtHc5UrfKSsLSED9MRh4ZWuNn2x5zEHEWKA2Ft6KfkdXzfyQ4VDwDPvPTwnLm7Lg53SMfLyh8",{"encoding": "jsonParsed","maxSupportedTransactionVersion":0}]}'

```
