# create fabric
fabric:create --clean --zookeeper-password admin --wait-for-provisioning
fabric:container-create-child root child 1
fabric:container-list

# stop fabric
fabric:container-stop child1
fabric:container-stop child2
shutdown -f

# rollback:
# remove and delete profile from container
fabric:container-remove-profile child2 mtc-fuse-test
fabric:profile-delete mtc-fuse-test

# creation:
# create a new mtc-fuse-test profile to contain the FuseTest feature
fabric:profile-create --parents feature-camel-jms mtc-fuse-test
# add feature repository to profile
profile-edit -r mvn:com.mycompany/FuseTestFeature/1.0.0-SNAPSHOT/xml/features mtc-fuse-test
# add camel-jms feature
profile-edit --features camel-jms mtc-fuse-test
# add FuseTestBasic feature to the profile
profile-edit --features FuseTestBasic mtc-fuse-test
# deploy mtc-fuse-test profile to container
fabric:container-change-profile child1 mtc-fuse-test



