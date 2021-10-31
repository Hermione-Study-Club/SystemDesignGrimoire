# Consistent hashing

## Problem

In scale out scenario, there's a common way to distribute data across multiple serves:

serverIndex = hash(key) % N

> N is a server pool size

In fixed pool size, this method should be fine, but it will encounter rehashing problem when server pool size get change.

## Target

Consistent hashing can be a solution to evenly distribute data even when server pool size got change.

## Basic elements
- key
- server
- hash space/ring
- hash function

## mechanism

- Key will find the first server in a clockwise direction
- Use same hash function to hash key and server to be evenly distributed in ring

## Virtual nodes
replicas of server point in ring

Through multiple server point/node in the ring to evenly distribute key to server.

## trade off

multi server point/node also need more space to store data.
