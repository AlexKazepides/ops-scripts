Purpose

Safely restarts the checkout-api service. The script defaults to a dry run for safety — it checks and prints the current service status but does not restart anything unless DRY_RUN is explicitly set to false in the script.

Usage

Run the script directly:

bash

./restart_service.sh

By default (DRY_RUN=true), the script will:

Print the current status of checkout-api via systemctl status.

Print a message stating it would restart the service, and instruct you to set DRY_RUN=false in the script to perform a real restart.

To perform an actual restart, edit the script and change:

bash

DRY_RUN=true

to:

bash

DRY_RUN=false

And save the changes made to the script

When DRY_RUN=false, the script will:

Print the current status of checkout-api.

Restart the service via systemctl restart.

Wait up to TIMEOUT seconds (default: 30) via sleep.

Print whether the service is active via systemctl is-active.

Prerequisites

sudo privileges, since systemctl status, systemctl restart, and systemctl is-active are all invoked with sudo.

The checkout-api service must be registered with systemctl on the host.

Troubleshooting

Status check fails or the service isn't found: The script runs systemctl status with || true, so a failed or missing-service status check will not stop the script from continuing — it will still proceed to the dry-run message or the restart logic.

Restart doesn't seem to take effect within expected time: The script only waits TIMEOUT seconds (default 30) before checking is-active; if the service normally takes longer to come up, the reported active/inactive state may not reflect its final status.

Script appears to do nothing on restart: Confirm DRY_RUN is set to false in the script — by default it is true, and the script will only print what it would do.

Script errors out or behaves unexpectedly: The script as shown does not include a closing fi for its if [ "$DRY_RUN" = true ]; then ... else ...  block, so it may be incomplete/truncated. Verify the full script has a matching fi before relying on it.