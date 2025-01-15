# Objective-C KVO: Delayed Crash with Weak References and Deallocated Observers

This repository demonstrates a subtle bug in Objective-C related to Key-Value Observing (KVO) and weak references.  The bug involves a delayed crash that can be difficult to reproduce and debug because it relies on the timing of object deallocation relative to KVO operations.

The `BuggyKVO.m` file contains the problematic code, while `FixedKVO.m` provides a corrected version.

## Problem

When using weak references in KVO, if the observed object is deallocated before the observer removes itself, subsequent attempts to observe changes can lead to crashes. These crashes aren't always immediate and are difficult to pinpoint during testing.

## Solution

The solution involves carefully managing the lifecycle of the observer and ensuring that `removeObserver:` is called before the observed object's deallocation, ideally in `-dealloc` of the observer or when the observer no longer needs to observe changes.