# django_extend_user_models


## 1- Models
```bash

# models.py

# Import

from django.contrib.auth.models import User
from django.db.models.signals import post_save
from django.dispatch import receiver




#----------------------------------- USER -->> PROFILE --------------------------------#

class Profile(models.Model):

    # Appel de user
    
    user = models.OneToOneField(User, on_delete=models.CASCADE, related_name='profile')  # 1 user <---> 1 profil
    
    # Champs suplementaires
    
    contacts = models.CharField(max_length=30, null=True)
    image = models.ImageField(upload_to='profile/', default='useravatar.png')
    birth_date = models.DateField(null=True)

    # Initialisation a la creation
    
    @receiver(post_save, sender=User)
    def create_user_profile(sender, instance, created, **kwargs):
        if created:
            Profile.objects.create(user=instance)

    @receiver(post_save, sender=User)
    def save_user_profile(sender, instance, created, **kwargs):
        
        instance.profile.save()

```


## 2 enregistrement



```bash

    # User
    user = User(username =username, first_name = first_name, last_name = last_name, email = email)
    user.save()
    
    # Profile
    
    user.profile.contacts = contacts
    user.profile.image = img
    user.profile.birth_date = birth_date

    user.save()
    
    # Password
    
    user.password = password
    user.set_password(user.password)
    user.save()


```

