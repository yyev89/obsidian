Ground rules:
* an AS must look like a single "thing" from the outside
	- internal routes should not be exposed
	- policy shoud apper the same from all edges
- whoever has the packet determines how it will exit their AS
- it should be possible to ask to enter an AS at a particular point
- it should be possible to ask for other special packet handling
- it should be possible to hide information when needed without impacting overall routing operation
- it should be possible to scale to hundreds of milliones of routes, paths etc.