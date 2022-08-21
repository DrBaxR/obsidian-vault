Tags: #maven #devops 
Created: 2022-08-21 17:08
References: [[Introduction to the POM]]

# Maven Goal
Maven phases (which is what [[Maven Build Lifecycle]]s are made out of) are, in turn, made out of goals. A plugin goal represents a task, which is finer than a build phase, that controbutes to building/managing the project.

Goals can be bound to one or more build phases. If a goal is bound to multiple build phases, it will be called in each of those phases.

Some goals can only be executed during a specific phase - for those you don't need to specify the phase in which you want them to be used. Other goals mat be used in many phases - for those you need to specify the phase in which you want to use them, using the `<phase>` element of the [[POM]].