                                                                            Scaling and Replication
---																			
**Replication -**
           - means we can replicate our pod by mathing with actual state and desired state by using replication controller and replica set. we can learn
more about those replication system from replica set and replication controller topic.

- we use replication beacuse if one pod fails then user can access server from another pod, it aslo helps us to manage traffic. as a example when a cricket match running in hotstar
then there are many many replication of pods where match is running, and one user accessing one pod.

**Scaling -**
        suppose there are 100 pods for 100 match viewer. if viewer increases then the pods will automatically increases, if viewer decreases then pod number
will automatically decreases. support we have 5 nodes and each node is capable to launch 20 pods, if all nodes reaches their quota then by using scaling 
the number of nodes will aslo increases.

**load balancing-**
             we can balance load or traffic by using replication and scaling.

