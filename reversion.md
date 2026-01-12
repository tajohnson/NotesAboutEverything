
select v.*,v.object_repr, dct.model,u.username,r.date_created  from reversion_version v left join reversion_revision r on v.revision_id=r.id left join auth_user u on r.user_id=u.id left join django_content_type dct on dct.id=v.content_type_id where v.object_repr = 'sandyindustries.com';


select v.object_repr, if(v.type=0, 'add', if(v.type=1, 'change', if(v.type=2, 'delete', 'oops'))) as state, dct.model,u.username,r.date_created  from reversion_version v left join reversion_revision r on v.revision_id=r.id left join auth_user u on r.user_id=u.id left join django_content_type dct on dct.id=v.content_type_id where v.object_repr = 'damongolding.com';
