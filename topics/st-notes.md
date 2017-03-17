# ST Notes

## Action Methods

* **isApplicable**: Return true is this action should be performed. Parameters come from its base class' ApplyAction method.
* **commonApply**: Adds the action to the action Repository \(which triggers sync\(\)\), and saves data to the corrisponding repository \(job, estimate, or location\). If you need to save to another repository, you overload it with that save, and then call the original commonApply like this: `JobAction.prototype.CommonApply.call(this, job);`



