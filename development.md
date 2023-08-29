# Configure nuget access

On windows environment
- Install nuget.exe

Add source on local list sources with access read use token
```bash
nuget.exe sources Add -Name "Binesty" -Source "https://nuget.pkg.github.com/read-packages/index.json" -username read-packages -password [token]
```


# Configure local rabbitmq with docker

run image local development to with docker

```bash
docker pull rabbitmq:3-management
docker run --rm -it -p 16672:15672 -p 6672:5672 rabbitmq:3-management
```