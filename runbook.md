Preconditions

You have sudo access on the host.

restart_service.sh is present and its if/else block closes properly (verify fi exists — see README Troubleshooting).


Steps

1. Open restart_service.sh in an editor.

2. Confirm SERVICE_NAME="checkout-api".

3. Set DRY_RUN=false. (It defaults to true, which will not restart anything.)

4. Save the file.

5. Run the script: ./restart_service.sh

6. Read the printed status output from systemctl status checkout-api (step runs before the restart).

7. Confirm the script prints "Restarting checkout-api..." — if it instead prints "[DRY RUN]", DRY_RUN was not saved as false. Return to step 3.

8. Wait for the script to finish sleeping (up to 30 seconds, per TIMEOUT).

9. Read the final output line from systemctl is-active checkout-api.

10. If the output is active, the restart succeeded. Stop here.

11. If the output is anything other than active, the service did not come up in time — escalate; the script has no further recovery logic beyond this check.

After the incident

Set DRY_RUN back to true in the script to restore the default safe state for the next run.