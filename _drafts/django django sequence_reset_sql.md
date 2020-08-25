https://stackoverflow.com/questions/43663588/executing-djangos-sqlsequencereset-code-from-within-python

https://docs.djangoproject.com/en/dev/ref/django-admin/#sqlsequencereset

https://www.postgresql.org/docs/9.5/functions-sequence.html

https://fleschenberg.net/moving-a-django-model-to-another-app/

sequence_sql = connection.ops.sequence_reset_sql(no_style(), [NewPricingCoupon, ])
    with connection.cursor() as cursor:
        for sql in sequence_sql:
            cursor.execute(sql)