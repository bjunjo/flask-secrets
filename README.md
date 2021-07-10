# flask-secrets
## Problem: Using WTF to make forms (using Jinja &amp; Flask)
## Solutions
1. Use WTF to make a web form
```
from flask_wtf import FlaskForm
from wtforms import StringField, PasswordField, SubmitField
from wtforms.validators import DataRequired, Email, Length

...

class MyForm(FlaskForm):
    email = StringField(label='Email', validators=[DataRequired(), Email()])
    password = PasswordField(label='Password', validators=[DataRequired(), Length(min=8)])
    submit = SubmitField(label="Log In")
```

2. Make a `login` function
```
@app.route("/login", methods=["GET", "POST"])
def login():
    form = MyForm()
    form.validate_on_submit()
    if form.validate_on_submit():
        if str(form.email.data) == "admin@email.com" and str(form.password.data) == "12345678":
            return render_template('success.html')
        else:
            return render_template('denied.html')
    return render_template('login.html', form=form)

```
3. Using Flask blocks to make a website faster and easier
```
{% extends "bootstrap/base.html" %}
{% import "bootstrap/wtf.html" as wtf %}
	{% block title %}Success{% endblock %}
{% block content %}
{% endblock %}
```
4. Using Bootstrap with a library
```
from flask_bootstrap import Bootstrap

# In HTML
{% extends "bootstrap/base.html" %}
{% endblock %}
```
5. 2 ways to make forms
```
# Long way
<form name="form" method="POST" action="{{ url_for('login') }}" novalidate>
  {{ form.csrf_token }}
  <p>
    {{ form.email.label }} <br> {{ form.email(size=30) }}
    {% for err in form.email.errors %}
    <span style="color:red">{{ err }}</span>
    {% endfor %}
  </p>
  <p>
    {{ form.password.label }} <br> {{ form.password(size=30) }}
    {% for err in form.password.errors %}
    <span style="color:red">{{ err }}</span>
    {% endfor %}
  </p>
  {{ form.submit }}
</form>

# Short way
  {{ wtf.quick_form(form) }}
```
# Lessons
1. Using Flask and blocks make things easier and build like legos
2. There's an easy way to do this but the trick is knowing what is going on behind the blackbox
