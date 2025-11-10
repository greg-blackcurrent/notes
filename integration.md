

I'm thinking about what needs to be done to ingest the data from bcedge.

```csharp
    public abstract record IngestEnergySharedModel
    {
        public int LocationId { get; set; }
        public int DeviceId { get; set; }
        public DateTime Received_Date { get; set; }
        public double Generation { get; set; }
        public double Import { get; set; }
        public double Export { get; set; }
        public double Load { get; set; }
        public double Charge { get; set; }
        public double Discharge { get; set; }
    }
```


Received_Date: Should this be the UTC datetime at which the modbus sample was made?

Each bcedge device will have a unique mqtt client-id, and post to a unique mqtt topic. i.e. `bcedgeapp/bcedge-dignam/sigen`

I assume we will need to configure a mapping from this topic or client-id to the platform device id, and location id.

How shall we handle this in the platform?

Perhaps we could put something in the calendar to discuss?

Ticket is here:
https://blackcurrent-io.atlassian.net/browse/BOC-613




