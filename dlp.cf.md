TODO - add max tflag multiple matches?  Add counts?

body DLP_US_SSN /\b(?!000|666|9)\d{3}([ -]?)(?!00)\d{2}\1([ -]?)(?!0000)\d{4}\b/
describe DLP_US_SSN Contains US Social Security Number
tflags DLP_US_SSN multiple
score DLP_US_SSN 0.01



body DLP_CC_VISA /\b((?:4\d{3})|(?:5[1-5]\d{2})|(?:6011)|(?:6601)|(?:3[68]\d{2})|(?:30[012345]\d))[ -]?(\d{4})[ -]?(\d{4})[ -]?(\d{4}|3[4,7]\d{13})\b/
describe DLP_CC_VISA Contains DLP Visa card number
tflags DLP_CC_VISA multiple
score DLP_CC_VISA 0.01

body DLP_CC_MASTERCARD /\b5[1-5]\d{2}([\ \-]?)\d{4}\1\d{4}\1\d{4}\b(?!([^<]+)?>)\b/
describe DLP_CC_MASTERCARD Contains DLP Mastercard card number
tflags DLP_CC_MASTERCARD multiple
score DLP_CC_MASTERCARD 0.01

body DLP_CC_DISCOVER /\b6(?:601|011|22(?:1(?=[\ \-]?(?:2[6-9]|[3-9]))|[2-8]|9(?=[\ \-]?(?:[01]|2[0-5])))|4[4-9]\d|5\d\d)([\ \-]?)\d{4}\1\d{4}\1\d{4}\b(?!([^<]+)?>)\b/
describe DLP_CC_DISCOVER Contains DLP Discovercard number
tflags DLP_CC_DISCOVER multiple
score DLP_CC_DISCOVER 0.01

body DLP_CC_JCB /\b35(?:2[89]|[3-8]\d)([\ \-]?)\d{4}\1\d{4}\1\d{4}\b(?!([^<]+)?>)\b/
describe DLP_CC_JCB Contains DLP JCB card number
tflags DLP_CC_JCB multiple
score DLP_CC_JCB 0.01

body DLP_CC_AMEX1 /\b3[47]\d{1,2}([\ \-]?)\d{6}([\ \-]?)\d{5}\b/
describe DLP_CC_AMEX1 Contains DLP Amexcard number
tflags DLP_CC_AMEX1 multiple
score DLP_CC_AMEX1 0.01

body DLP_CC_AMEX2 /\b3[47]\d{1,2}([\ \-]?)\d{4}([\ \-]?)\d{4}([\ \-]?)\d{3}\b/
describe DLP_CC_AMEX2 Contains DLP Amex card number
tflags DLP_CC_AMEX2 multiple
score DLP_CC_AMEX2 0.01

