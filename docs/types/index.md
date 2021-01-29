---
title: OT Types
nav_order: 4
layout: default
---

# OT Types

ShareDB provides a realtime collaborative platform based on [Operational Transformation (OT)](https://en.wikipedia.org/wiki/Operational_transformation). However, ShareDB itself is only part of the solution. ShareDB provides a lot of the machinery for handling ops, but does not provide the actual implementation for transforming ops.

Transforming and handling ops is delegated to an underlying OT type.

ShareDB ships with a single, default type -- [`json0`](https://www.npmjs.com/package/ot-json0).

## Installing other types
