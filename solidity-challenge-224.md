solidity-challenge-224.md

# Calyptus Solidity Challenge 224
*Which of the following are not keywords in solidity and why?*
`
uint 1 == 1 seconds
1 minutes == 60 seconds
1 hours == 60 minutes
1 days == 24 hours
1 weeks == 7 days
1 months == 4 weeks
1 years == 12 months
`
# Answer

years and months are not keywords in Solidity. They are deprecated. 

In Solidity, the following are not keywords:

1. uint 1 == 1 seconds: This is not a valid statement in Solidity. uint is a keyword, but 1 is not. The correct way to declare a uint variable is uint variableName = value;. Also, seconds is not a keyword, but it is a time unit.

2. 1 months == 4 weeks: This is not a valid statement in Solidity. months and weeks are time units, but they are not equivalent in Solidity. In Solidity, 1 months is assumed to always be exactly 30 days, which is not necessarily equivalent to 4 weeks.

3. 1 years == 12 months: This is not a valid statement in Solidity. years and months are time units, but they are not equivalent in Solidity. In Solidity, 1 years is assumed to always be exactly 365 days, which is not necessarily equivalent to 12 months.

The rest (1 minutes, 1 hours, 1 days, 1 weeks) are valid time units in Solidity. They are used to convert time units to seconds. For example, 1 minutes is equivalent to 60 seconds, 1 hours is equivalent to 3600 seconds, 1 days is equivalent to 86400 seconds, and 1 weeks is equivalent to 604800 seconds.

