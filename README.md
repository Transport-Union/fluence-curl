


have peer running

test the call you want to make: 

docker exec -ti fluence-peer-123xxx curl -X POST ...

confirm location of curl binary: 

docker exec -ti fluence-peer-123xxx which curl 

confirm the binary is whitelisted on your peer. 

in Config.toml 

allowed_binaries = [
    "/usr/bin/curl",
]


fluence service new tuCurl 

cat 
