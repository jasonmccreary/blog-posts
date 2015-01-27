---
title: Generate user password for CakePHP Auth
author: Jason McCreary
excerpt: A script that hashes password for existing user records when using CakePHP Auth.
layout: post
comments: true
permalink: /2012/03/generate-user-password-cakephp-auth/
categories:
  - Main Thread
tags:
  - cakephp
  - script
description: A script that hashes password for existing user records when using CakePHP Auth.
---
The other day I imported some existing user accounts into my CakePHP project. The project was using [CakePHP&rsquo;s Auth Component][1]. By default, CakePHP Auth uses its own hashing to encrypt the password. So I needed to hash the password for the existing user accounts.

I vaguely remembered a script in the CakePHP Book, but couldn&rsquo;t find it. I figured I could have searched for one or write one. In typical developer fashion, I chose the latter.

    public function generate_passwords() {
        // get the users that need their password hashed
        $results = $this->User->find('all', array('recursive' => -1, 'fields' => array('User.id', 'User.passwd'), 'conditions' => array('User.email LIKE' => '%@example.com', 'NOT' => array('User.passwd' => null))));
    
        $count = count($results);
        foreach ($results as $result) {
            $result['User']['passwd'] = $this->Auth->password($result['User']['passwd']);
    
            if (!$this->User->save($result)) {
                echo 'Could not update account for User.id = ', $result['User']['id'], PHP_EOL;
                --$count;
            }
        }
    
        echo 'Updated ', $count, ($count == 1 ? ' record.', 'records.'),  PHP_EOL;
        exit;
    }
    

Paste this code into any controller that has access to the Auth component and your Auth User model. Then visit the URL. In this case */users/generate_passwords*. **Only run this once**. Otherwise, it will double hash your passwords.

You should only need to adjust the `conditions` option for the `find()` method so that it only returns the user accounts that need their passwords hashed. Also note that this code is built for CakePHP 1.3. However, it should port pretty easily to 2.0. If you do so please let me know.

 [1]: http://book.cakephp.org/1.3/view/1250/Authentication
