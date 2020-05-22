+++
title = "Revert the routing"
chapter = false
weight = 80
+++

The support department is receiving calls from customers about errors they are getting after the new release. You want to put the routing back to a 50:50 split.

This can be done in 2 ways:

* Revert the last commit and force push - rewrites history
* Fix forward - by updating the file and committing/push

If you revert the change you will lose the history of the routing changes. Many companies prefer a **Fix Forward** approach because of this.

We will revert the change and push just to demonstrate this approach. Run the following commands from your second terminal window (make sure you leave the other terminal window running the query):

```bash
# Undo last commit and don't keep the changes
git reset --hard HEAD~1

# Force push the changes so Flux picks them up
git push -f
```

Go back to the original terminal window. In about a minutes you will see the output change and the **hostname** will show **v1** and **v2**.

Once you happy boht version are being called go back to the second terminal window and enter the following command:

```bash
git log --oneline
```

You will see that there is no history of the change where 100% traffic went to **v2**.
