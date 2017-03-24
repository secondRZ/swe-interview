# ST Notes

## Visual Studio

* Run: Open the executable.
* Set Startup Project: Telling visual studio which project to run first in a multi-project solution.
* Build: Compile and create executable.
* Clean: 
* Rebuild = Clean + Build. For a multi-project solution, "rebuild solution" does a "clean" followed by a "build" for each project \(possibly in parallel\). Whereas a "clean solution" followed by a "build solution" first cleans all projects \(possibly in parallel\) and then builds all projects \(possibly in parallel\). This difference in sequencing of events can become significant when inter-project dependencies come into play.

### Nuget

* Nuget is a package manager like npm. A central repository where C\# libraries live, and a way to manage your dependencies. 
* To add a new package, right click on References and click "Manage Nuget Packages"
* It's smart to use the same version of a package for all projects in a solution. If you diverge somehow, there's a section in the package manager call "Consolidate" that helps you figure it out.

## Front-end

### Getting New Data

* Downloading Pricebook: Some data is not updated often, because it doesn't need to be. If there is data that hasn't been updated, try downloading the pricebook.
* Quitting the app is also like clearing the cache. That can also help.

### Overriding Action Methods

* **IsApplicable**: Return true if this action should be performed. Parameters come from its base class' ApplyAction method.
* **InnerApply**: What you actually want to do with the data. Parameters come from its base class' ApplyAction method. If everything is done in the back-end, just write a blank function. \(See delete-estimate.js\).
* **CommonApply**: Adds the action to the action Repository \(which triggers sync\(\)\), and saves data to the corrisponding repository \(job, estimate, or location\). If you need to save to another repository, you overload it with that save, and then call the original commonApply like this: `JobAction.prototype.CommonApply.call(this, job);`Parameters come from its base class' ApplyAction method.
* **Reapply**: Don't do anything with this. It's just an apply that doesn't trigger the "Job is not found" alert. We use this to apply non-synchronized actions after synchronization. This could happen if a sync took a long time and the tech performed actions while it was happening.
* **this.ToServerIds**: "If this action works with an object that will receive a server id later."



