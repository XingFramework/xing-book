# Error Documents

_a dialogue_


Error documents always include only objects - if the corresponding field has an array value, the error field has { 3: { type, message } }
And: A parent object's errors supersede its children's.
If you have:
{
    parent: {
        child: {
        }
}
And there is both an error in child and parent, parent has type,message in the error document and may or may not include child.
Unresolved: what if the data response would look like
{
    might_have_an_error: {
        type: urgent
        message: A message in a chat program
    }
}
i.e. normal data with "type" and "message" fields
I'd like feedback on all this.

Evan
On the array errors:  my instinct would be a nil-padded array, (i.e. [ nil, nil { type: '', message: ''} ] though i recognize that's also problematic.  But if the goal is to have the error structure mirror the data structure so they can be processed by the same code, replacing an array with an object strikes me as likely to be a longer ongoing headache.

But I don't care enough about that to be an advocate for the position beyond that statement.
the 2nd issue (error in both child and parent) I'm not following from the description.

Judson
I see your point re arrays but: JS mostly treats arrays as objects with "0", "1", "2"... keys anyway.

Evan
not 0, 1, 2 keys?

Judson
Parent errors superseding child mostly comes from those two issues
Nope: spec says you keep an integer counter and then <<get>> toString(counter) e.g. to iterate
Dumb, huh?

Evan
JS: no comment necessary.

Judson
The two wrinkles here are:
If error documents have arrays in them, what happens if there's an error on the array?

Evan
i see.  meaning both 'array too long' and 'third element misformatted'

Judson
How do we handle ambiguity between type/message field names if the normal data has those fields?
Exactly.

Evan
if you report { type: 'lengthError', message: 'array too long'} there's nowhere to put the child error.

Judson
Since arrays *can't* have a "type" field, the error document has to have an object there.
But you could say
{
    type: "le"
    message: "a2l"
    2: {
        type: misformatted
        }

Evan
yeah

Judson
Talking this through, the field name ambiguity I think is the only real problem.

Evan
just curious: what prior art exists on returning errors about JSON resources?
we can't be the only people to have ever thought about this

Judson
I can say with 90% confidence we're not and I've read some of the thoughts.
And everyone comes up with this.
OH! Wait:
If data document has e.g. a type field and there's no error, it doesn't appear in the error document.
Even if there's an error for a value object and on the values "type" field, you can tell the difference because an error report looks like { type, message } and an error report type field looks like "string"
 {
    has_an_error: {
        type: "oops"
        message: "you screwed up"
    }
}
vs
 {
    has_a_value_with_error: {
        type: {
            type: "double oops"
        }
    }
}
In that one case, the server has to choose one error to report, but I think that's reasonable.
What do you think?

Evan
sorry, those lost me.  let me re-read and see if i can grok.
by "if a data document has e.g. a type field" you mean if a resource literally has a key named "type"?
