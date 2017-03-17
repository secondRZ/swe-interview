# ST Notes

## Action Methods

* **IsApplicable**: Return true is this action should be performed. Parameters come from its base class' ApplyAction method.
* **InnerApply**: What you actually want to do with the data. Parameters come from its base class' ApplyAction method. If everything is done in the back-end, just write a blank function. \(See delete-estimate.js\).
* **CommonApply**: Adds the action to the action Repository \(which triggers sync\(\)\), and saves data to the corrisponding repository \(job, estimate, or location\). If you need to save to another repository, you overload it with that save, and then call the original commonApply like this: `JobAction.prototype.CommonApply.call(this, job);`Parameters come from its base class' ApplyAction method.
* **Reapply**: Don't do anything with this. It's just an apply that doesn't trigger the "Job is not found" alert. We use the to apply non-synchronized actions after synchronization. This could happen if
* **this.ToServerIds**: "If this action works with an object that will receive a server id later."



