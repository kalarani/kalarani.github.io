+++
date = '2011-04-11T08:30:42+05:30'
title = 'Model form - Get it working'
+++
Started playing around with python and django last week. There is a lot of stuff that django provides us to build a basic web app. Despite its very good documentation, I found it hard to get the model form working. Model forms are useful when you have a basic model with some fields defined and you need a form to either add or edit the model. Writing views for such models is redundant, since both of them are going to contain the same information anyway and its prone to human error. Model form comes in handy in such situations.

Lets get started with a basic model that holds an item, its price and the date when it was added:

```python
from django.db import models
 
class Transaction(models.Model):
 item = models.CharField(max_length=200)
 price = models.IntegerField()
 date_added = models.DateTimeField('date added')
```

A model form for the above would look like:

```python
from django import forms

class TransactionForm(forms.ModelForm):
        class Meta:
                model = Transaction
        date_added = forms.DateField(initial=datetime.date.today)
```

To display the model form in a view, we need to have a html file that'll be used to render the model form that is passed as a parameter to that. Using django's shortcuts we can render the form using the code below:

```python
def index(request):
        return render_to_response('managExpenses/transaction_form.html', {'form':TransactionForm}, context_instance=RequestContext(request))
```

If you are using django's generic views, then the code will look like:

```python
def index(request):
        return create_object(request, form_class=TransactionForm)
```

In the above code, the create_object method will look for the transaction_form.html placed in the templates folder and use it to render the form.

The transaction_form.html looks something like:

```html
<form action="/some/url/" method="post">
{% csrf_token %}
{{form}}
<input type="submit" value="Submit"/>
</form>
```

The model form is replaces the {{form}} in the above html. The csrf_token is used by the CSRF (Cross Site Request Forgery) protection and needs to be included in every form that posts to your internal url. 

You can now go ahead and modify the urls.py to assign a url pattern to the above view and see it working.
