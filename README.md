# Basic configuration of Loki with .net 8 api.

## Step 1 :

### Run followinfg command in Windows :

```
loki-windows-amd64.exe -config.file=configureLoki.yaml
```

## Step 2 :

### Create And Connect Loki with .net 8 application.

Add Following Package :

```
 <PackageReference Include="Serilog" Version="4.0.2" />
 <PackageReference Include="Serilog.AspNetCore" Version="8.0.3" />
 <PackageReference Include="Serilog.Sinks.Grafana.Loki" Version="8.3.0" />
```

Add Logger to .net core Application

```
// Add services to the container.
Log.Logger = new LoggerConfiguration()
    .WriteTo.GrafanaLoki(
    uri : "http://localhost:3100",
    labels: new List<LokiLabel>() { new LokiLabel() { Key = "ApplicationName",Value = "LokiAPIEnd" } },
    propertiesAsLabels : new List<string>() { "Host" })
    .CreateLogger();

builder.Logging.AddSerilog(Log.Logger);
```

use Logger to log :

```
_logger.LogError(new Exception("Invalid"), "Some thing went wrong");
```

## Step 3 :

### Connect loki to Grafana

open grafana login and open :
**Connections->Data Source->Add new DataSource**
select **Loki-> Add Connection String URl** for Current Example [localhost:3100](http://localhost:3100/)

Done

All Application logs will be visible to grafana

From **Explore-> Select Outline source -> loki**
and query

Reference for Debugging that worked :

1. [Configure loki](https://grafana.com/docs/loki/latest/configure/)
2. [Congfigure Storage](https://grafana.com/docs/loki/latest/configure/storage/)
3. [Loki HttpAPi](https://grafana.com/docs/loki/latest/reference/loki-http-api/)
4. [Grafana not connectin to loki -> time out Error](https://github.com/grafana/grafana/issues/42801)

### Some API to quick check Loki

1. [Check Service](http://localhost:3100/services)

2. [Check loki status](http://localhost:3100/ready)
