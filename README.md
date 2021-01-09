<div align="center">

<h1>🧚‍♀️ Pixie</h1>
<i>"It's like systemctl, but online"</i>   
  
<code>com.abyssaldev.pixie</code>

An application server-side management daemon & service mesh, written in Kotlin  
Runs on top of `systemctl`.  
Features a RESTful control API, a web interface, and live CRUD operations.  
There's a CLI too.  
Super simple to use and even easier to maintain.  
Adapted from internal tooling at GalEdu.

</div>

### What can Pixie do?
**Pixie can:**
- Manage your application process(es)
- Connect them to each other, loosely-coupled (processes are unaware of other processes existing)
- Dynamically handle process drop-in drop-out (client disconnect, process crashes, scale up/down), without restarting all clients
- Report process crashes to an exception handling service
- Fault detection for otherwise non-fault-resistant runtimes (used extensively with the JVM)
- Replace Docker/Kubernetes for small-scale setups (where D/K8S would be *overkill*)

**Pixie can't:**
- Route network requests outside Pixie clusters (`nginx` recommended)
- Act as a firewall between Pixie clients & clusters
- Isolate Pixie clients (coming soon)
- Replace a well-configured Docker/Kubernetes setup for big projects
- Make your breakfast (do it yourself)
- Run on Windows/Windows Server

### Example usage
**CLI**
```console
abyss@cpu:~$ pixie client create-spec --file=abyss-client.json --run-now=false
== pixie 0.10.2 on cpu @ 1:08AM Sun 10 Jan NZDT ==
pixie-json: opened `abyss-client.json`
pixie-client-create: created client "Abyss Pixie Client" (use --debug to print client info)
pixie: finished in 652 milliseconds
== pixie ending ==
abyss@cpu:~$ pixie start abyss-client
== pixie 0.10.2 on cpu @ 1:09AM Sun 10 Jan NZDT ==
pixie-client-connect: connected to pixie mesh on localhost:11808
pixie-client-connect: sent START_CLIENT message for abyss-client
pixie-client-connect: received acknowledgement
== pixie ending ==
abyss@cpu:~$ pixie logs abyss-client --follow
== pixie 0.10.2 on cpu @ 1:09AM Sun 10 Jan NZDT ==
pixie-client-logs: beginning log follow for abyss-client
...
```
`abyss-client.json`
```json
{
  "meta-pixie-version": "0.10.2",
  "clients": {
    "abyss": {
      "name": "Abyss Pixie Client",
      "description": "Abyss' managed instance on the Pixie platform",
      "actions": {
        "start": {
          "depends": ["abyss-compile"],
          "script": "./gradlew run"
        },
        "stop": {
          "script": "pixie pman kill-all abyss"
        },
        "restart": {
          "depends": ["abyss-stop", "abyss-start"]
        }
      }
    }
  }
}
```


### Who uses this? Why?
(I don't really know why. For fun?)
* The [Abyss](https://github.com/abyssal/abyss) bot project
* Internal GalEdu tools

### Disclosure
Pixie is alpha-quality software and shouldn't be used in production.  
(I mean, none of my code should really be used in production, but go figure?)  
Abyssal ain't responsible for data loss, system loss, mental instability, collapse of the world, or your computer exploding.

### Copyright
Copyright &copy; 2019-2021 Abyssal and contributors, under the [MIT License](LICENSE.md).  
Aspects of Pixie's service mesh design are inspired by 
