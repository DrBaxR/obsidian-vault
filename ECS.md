Tags: #gamedev
Created: 2022-12-12 14:12
References: https://bevy-cheatbook.github.io/programming/ecs-intro.html

# ECS
ECS is an idiom or pattern that is used a lot in [[Game Development]].

Conceptually, ECS can be though of as an analogy to tables: The different data types (*Components*) are something like columns of a table and there can be an arbitrary number of rows (*Entities*) that contain values/instances of components.

An entity acts something like a storage of components, each of the components containing some fields that store data. Besides entities and components, there are systems, which are the things that contain the logic that operate with the component's data.