Who made the changes to a resource?

mysql> select v.type,  r.date_created, u.email , v.serialized_data from reversion_version as v left join reversion_revision as r on v.revision_id=r.id left join auth_user as u on r.user_id=u.id where v.content_type_id=57 and v.object_id = 71072\G


select object_repr from reversion_version where object_repr like "%@aurora.com" and type=1;


select object_repr from reversion_version where content_type_id=45 and object_repr like "%@aurora.com";