# reproducer-03541780
---
1. Create the needed resources to test the issue (`Namespace`, `Task`, `Pipeline`, `EventListener`, `TriggerBinding`, `TriggerTemplate`):
   ```sh
   oc apply -k https://github.com/gmeghnag/reproducer-03541780
   ```

2. Expose the `EventListener` to create a `Route`:
   ```sh
   oc expose svc/el-greetings-el -n reproducer-03541780
   ```

3. Set the `EventListener` replicas count to 2:
   ```
   oc scale deployment.apps/el-greetings-el --replicas=2 -n reproducer-03541780
   ```

4. Execute a POST request to trigger a new `PipelineRun` (run it twice to test both the EventListeners replicas):
   ```sh
   curl -X POST -H "Content-Type: application/json" --data '{"FIRST_GREETING": "TEKTON"}' "http://$(oc get route el-greetings-el -n reproducer-03541780 -o jsonpath='{.spec.host}')"
   ```
