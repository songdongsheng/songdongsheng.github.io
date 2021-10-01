---
title: ULID and TSID
excerpt: universally unique lexicographically sortable identifier (ULID), trend sorted identifier generator (TSID)
date: 2019-05-18 15:22:19
tags:
  - Java
categories: [Programming, Java]
---

# ULID and TSID

## ULID

[UUID](https://tools.ietf.org/html/rfc4122) can be suboptimal for many uses-cases because:

- It isn't the most character efficient way of encoding 128 bits of randomness
- UUID v1/v2 is impractical in many environments, as it requires access to a unique, stable MAC address
- UUID v3/v5 requires a unique seed and produces randomly distributed IDs, which can cause fragmentation in many data structures
- UUID v4 provides no other information than randomness which can cause fragmentation in many data structures

Instead, herein is proposed [ULID](https://github.com/alizain/ulid):

- 128-bit compatibility with UUID
- 1.21e+24 unique ULIDs per millisecond (48 bit ms + 80 bit randomness)
- Lexicographically sortable!
- Canonically encoded as a 26 character string, as opposed to the 36 character UUID
- Uses Crockford's base32 for better efficiency and readability (5 bits per character)
- Case insensitive
- No special characters (URL safe)
- Monotonic sort order (correctly detects and handles the same millisecond in the same generator instance)

## TSID

[TSID](https://github.com/songdongsheng/tsid) is a general-purpose trend sorted identifier generator, it is the variant version of ULID, since ULID is often overkill. TSID don't strict guaranty sort order in the same millisecond or microsecond.

- 48 bit ms + 32 bit randomness = VARCHAR(16), 8919 year
- 58 bit us + 42 bit randomness = VARCHAR(20), 9133 year
- 58 bit us + 62 bit randomness = VARCHAR(24), 9133 year
- 48 bit ms + 80 bit randomness = VARCHAR(26), 8919 year
