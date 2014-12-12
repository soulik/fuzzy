Fuzzy Inference System (FIS) for Lua
=====

Simple Fuzzy Inference System completely written in Lua.

It uses discrete steps (of size 0.01) to get final fuzzy set.
Defuzzyfication uses centroid approach to obtain output value.

There are three objects to use:
* F - Fuzzy set
* L - Membership function
* R - FIS Rule

Membership functions can use one of these predefined math functions:
* fuzzy.gauss
* fuzzy.trapezoid
* fuzzy.triangle

Rules can be defined with fuzzy logic operators: AND *, OR +, NOT -
```lua
	local rule = R(1)
	rule.premise = (service['poor'] * food['rancid']) + (-service['excelent'])
	rule.implication = tip['cheap']
```

You can obtain output values by calling fuzzy system object with a set of input values.

Dependencies
============
* Lua 5.1.x or LuaJIT 2.0+

Usage
=====

## Tipper example

```lua
local fuzzy = require 'fuzzy'

do
	local f1 = fuzzy.solver()
	local L = f1.L
	local F = f1.F
	local R = f1.R
	
	local service = F {'service', {0, 10}}
	service['poor'] = L {fuzzy.gauss, {1.5, 0}}
	service['good'] = L {fuzzy.gauss, {1.5, 5}}
	service['excelent'] = L {fuzzy.gauss, {1.5, 10}}

	local food = F {'food', {0, 10}}
	food['rancid'] = L {fuzzy.trapezoid, {0, 0, 1, 3}}
	food['delicious'] = L {fuzzy.trapezoid, {7, 9, 10, 10}}

	local tip = F {'tip', {0, 30}}
	tip['cheap'] = L {fuzzy.triangle, {0, 5, 10}}
	tip['average'] = L {fuzzy.triangle, {10, 15, 20}}
	tip['generous'] = L {fuzzy.triangle, {20, 25, 30}}

	local r1 = R(1)
	r1.premise = service['poor'] + food['rancid']
	r1.implication = tip['cheap']

	local r2 = R(1)
	r2.premise = service['good']
	r2.implication = tip['average']

	local r3 = R(1)
	r3.premise = service['excelent'] + food['delicious']
	r3.implication = tip['generous']

	local values = {
		service = 8.0,
		food = 6.5,
	}

	-- @22.2115
	local result = f1(values)

	print(result)

end
```

Copying
=======
Copyright 2013, 2014 Mário Kašuba
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are
met:

* Redistributions of source code must retain the above copyright
  notice, this list of conditions and the following disclaimer.
* Redistributions in binary form must reproduce the above copyright
  notice, this list of conditions and the following disclaimer in the
  documentation and/or other materials provided with the distribution.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
"AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY
THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
(INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
