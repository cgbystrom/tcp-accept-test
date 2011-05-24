Simple pathological test server designed to reproduce bad TCP accept() performance.
Often observed in virtualized servers (Xen among others).

There's a Server Fault question asked about this particular topic, see:
http://serverfault.com/questions/272483/why-is-tcp-accept-performance-so-bad-under-xen

To run this test:

 1. Install Java and Apache Bench
 2. Download the JAR from the downloads section
 3. Run
    `java -server -jar tcp-accept-server-1.0.0.jar`
 4. Warm up the JIT with keep-alive on `ab -n 100000 -c 10 -k http://127.0.0.1:8080/`
 5. Execute the test<br>
    `ab -n 100000 -c 20 http://127.0.0.1:8080/`<br>
    On a modern, decent machine you should see +25000 req/s.
    But on a virtualized server (Xen) this always is below 10k req/s.

Some more performance numbers regarding this behavior can be found at https://gist.github.com/985475 (uses netperf instead)