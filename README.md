


have peer running

test the call you want to make: 

docker exec -ti fluence-peer-123xxx curl -X POST ...

confirm location of curl binary: 

docker exec -ti fluence-peer-123xxx which curl:

/usr/bin/curl

confirm the binary is whitelisted on your peer. 
i.e hardware owner permits usage to workers. 

in Config.toml 

allowed_binaries = [
    "/usr/bin/curl",
]


fluence service new tuCurl 

in src/services/tuCurl/modules/tuCurl/module.yaml create the alias for path to the binary

mountedBinaries:
  curl: /usr/bin/curl

  i.e. curl is the name you can use within marine to call /usr/bin/curl 

in main.rs add interface to use the alias within your rust service

use marine_rs_sdk::marine;
use marine_rs_sdk::module_manifest;
use marine_rs_sdk::MountedBinaryResult;

#[marine]
#[link(wasm_import_module = "host")]
extern "C" {
    pub fn curl(cmd: Vec<String>) -> MountedBinaryResult;
}

construct your call as a vector of strings: 

  let curl_args = vec![
        String::from("-s"),
        String::from("-X"),
        String::from("POST"),
        etc ..,
        etc ...  
    ];

let response = curl(curl_args);

responses come back as either sdtout or sterror. you can convert bytes to strings like this: 

 let error = String::from_utf8(response.stderr).unwrap();  
 let result = String::from_utf8(response.stdout).unwrap();

  return result from rust function 

  deploy worker

in aqua call the worker

res: *string

for w <- workers:
        on w.workerId via w.hostId:
            res <- TuCurl.fetch()
<- res

make sure you have correctly typed the service 

service TuCurl("tuCurl"):
  fetch() -> string



  

  
