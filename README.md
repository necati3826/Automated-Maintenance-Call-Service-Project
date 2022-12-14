<h2>Use Case : </h2> 
There are two kinds of people in this world: those who would travel in a recreational vehicle (RV) and those who wouldn’t. The RV community is increasing exponentially across the globe. Over the past few years, HowWeRoll Rentals, the world’s largest RV rental company, has increased its global footprint and camper fleet tenfold. HowWeRoll offers travelers superior RV rental and roadside assistance services. Their tagline is, “We have great service, because that’s How We Roll!” Their rental fleet includes every style of camper vehicle, from extra large, luxurious homes on wheels to bare bones, retro Winnebagos.

You have been hired as the lead Salesforce developer to automate and scale HowWeRoll’s reach. For travelers, not every journey goes according to plan. Unfortunately, there’s bound to be a bump in the road at some point along the way. Thankfully, HowWeRoll has an amazing RV repair squad who can attend to any maintenance request, no matter where you are. These repairs address a variety of technical difficulties, from a broken axle to a clogged septic tank.

As the company grows, so does HowWeRoll’s rental fleet. While it’s great for business, the current service and maintenance process is challenging to scale. In addition to service requests for broken or malfunctioning equipment, routine maintenance requests for vehicles have grown exponentially. Without routine maintenance checks, the rental fleet is susceptible to avoidable breakdowns.

That’s where you come in! HowWeRoll needs you to automate their Salesforce-based routine maintenance system. You’ll ensure that anything that might cause unnecessary damage to the vehicle, or worse, endanger the customer is flagged. You’ll also integrate Salesforce with HowWeRoll’s back-office system that keeps track of warehouse inventory. This completely separate system needs to sync on a regular basis with Salesforce. Synchronization ensures that HowWeRoll’s headquarters (HQ) knows exactly how much equipment is available when making a maintenance request, and alerts them when they need to order more equipment.
<h2>Standard Objects</h2>
You’ll be working with the following standard objects:

Maintenance Request (renamed Case) — Service requests for broken vehicles, malfunctions, and routine maintenance.
Equipment (renamed Product) — Parts and items in the warehouse used to fix or maintain RVs.
<h2>Custom Objects</h2>
Vehicle — Vehicles in HowWeRoll’s rental fleet.
Equipment Maintenance Item — Joins an Equipment record with a Maintenance Request record, indicating the equipment needed for the maintenance request.
<h2>Business Requirements : </h2> 
This section represents the culmination of your meetings with key HowWeRoll stakeholders. It’s your blueprint to programmatically automate the support and maintenance side of their business.

<h2>Automate Maintenance Requests</h2>
With the exponential increase in RV popularity worldwide, HowWeRoll is supplying hundreds more luxury and economy vehicles around the globe. Along with this increase in their rental stock comes an inevitable increase in equipment failure. HowWeRoll has an existing process to handle these failures, but they want you to build an automation for their routine maintenance. You’ll build a programmatic process that automatically schedules regular checkups on the equipment based on the date that the equipment was installed.When an existing maintenance request of type Repair or Routine Maintenance is closed, create a new maintenance request for a future routine checkup. This new maintenance request is tied to the same Vehicle and Equipment Records as the original closed request. This new request's Type should be set as Routine Maintenance. The Subject should not be null and the Report Date field reflects the day the request was created. Remember, all equipment has maintenance cycles.

Calculate the maintenance request due dates by using the maintenance cycle defined on the related equipment records. If multiple pieces of equipment are used in the maintenance request, define the due date by applying the shortest maintenance cycle to today’s date.

Design the code to work for both single and bulk maintenance requests. Bulkify the system to successfully process approximately 300 records of offline maintenance requests that are scheduled to import together. For now, don’t worry about changes that occur on the equipment record itself.

Also expose the logic for other uses in the org. Separate the trigger (named MaintenanceRequest) from the application logic in the handler (named MaintenanceRequestHelper). This setup makes it simpler to delegate actions and extend the app in the future.
<h2>Synchronize Inventory Management</h2>
In addition to equipment maintenance, design HowWeRoll’s inventory data synchronization with the external system in the equipment warehouse. Although the entire HQ office uses Salesforce, the warehouse team still works on a separate legacy system. With this integration, the inventory in Salesforce updates after the equipment is taken from the warehouse to service a vehicle.

Write a class that makes a REST callout to an external warehouse system to get a list of equipment that needs to be updated. The callout’s JSON response returns the equipment records that you upsert in Salesforce. Beyond inventory, ensure that other potential warehouse changes carry over to Salesforce. Your class maps the following fields:

Replacement part (always true)

Cost

Current inventory

Lifespan

Maintenance cycle

Warehouse SKU

Use the warehouse SKU as the external ID to identify which equipment records to update within Salesforce.

Although HowWeRoll is an international company, the remote offices follow the lead of the HQ’s work schedule. Therefore, all maintenance requests are processed during HQ’s normal business hours. You need to update Salesforce data during off hours (at 1:00 AM). This logic runs daily so that the inventory is up to date every morning at HQ.
<h2>Create Unit Tests</h2>
Test your code to ensure that it executes correctly before deploying it to production.

First, test the trigger to ensure that it works as expected. Follow best practices by testing for both positive use cases (when the trigger needs to fire) and negative use cases (when the trigger must not fire). Of course, passing a test doesn’t necessarily mean you got everything correct. So add assertions into your code to ensure you don’t get false positives. For your positive test, assert that everything was created correctly, including the relationships to the vehicle and equipment, as well as the due date. For your negative test, assert that no work orders were created.

As mentioned previously, the huge wave of maintenance requests could potentially be loaded at once. The number will probably be around 200, but to account for potential spikes, pad your class to successfully handle at least 300 records. To test this, include a positive use case for 300 maintenance requests and assert that your test ran as expected.
