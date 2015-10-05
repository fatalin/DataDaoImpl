package com.publishing.dao;

import java.util.Date;
import java.util.List;

import org.hibernate.Criteria;
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.criterion.Disjunction;
import org.hibernate.criterion.MatchMode;
import org.hibernate.criterion.Restrictions;
import org.springframework.beans.factory.annotation.Autowired;

import com.publishing.model.Article;

public class DataDaoImpl implements DataDao {

	@Autowired
	SessionFactory sessionFactory;

	Session session = null;
	Transaction tx = null;

	@Override
	public boolean addEntity(Article article) throws Exception {

		session = sessionFactory.openSession();
		tx = session.beginTransaction();
		session.save(article);
		tx.commit();
		session.close();

		return false;
	}

	@Override
	public boolean updateEntity(Article article) throws Exception {

		session = sessionFactory.openSession();
		tx = session.beginTransaction();
		session.update(article);
		tx.commit();
		session.close();

		return false;
	}

	@Override
	public boolean deleteEntity(long id)
			throws Exception {
		session = sessionFactory.openSession();
		Object o = session.load(Article.class, id);
		tx = session.getTransaction();
		session.beginTransaction();
		session.delete(o);
		tx.commit();
		return false;
	}

	@Override
	public Article getEntityById(long id) throws Exception {
		session = sessionFactory.openSession();
		Article article = (Article) session.load(Article.class,
				new Long(id));
		tx = session.getTransaction();
		session.beginTransaction();
		tx.commit();
		return article;
	}

	@SuppressWarnings("unchecked")
	@Override
	public List<Article> getEntityList() throws Exception {
		session = sessionFactory.openSession();
		tx = session.beginTransaction();
		List<Article> articleList = session.createCriteria(Article.class)
				.list();
		tx.commit();
		session.close();
		return articleList;
	}

	@SuppressWarnings("unchecked")
	@Override
	public List<Article> getEntityListByAuthor(String author) throws Exception {
		session = sessionFactory.openSession();
		tx = session.beginTransaction();
		Criteria criteria = session.createCriteria(Article.class);
		Disjunction matchDisjunction = Restrictions.disjunction();
		matchDisjunction.add(Restrictions.like("authors", author,
				MatchMode.ANYWHERE));
		criteria.add(matchDisjunction);
		List<Article> articleList = criteria.list();
		tx.commit();
		session.close();
		return articleList;
	}

	@SuppressWarnings("unchecked")
	@Override
	public List<Article> getEntityListByKeyword(String keyword)
			throws Exception {
		session = sessionFactory.openSession();
		tx = session.beginTransaction();
		Criteria criteria = session.createCriteria(Article.class);
		Disjunction matchDisjunction = Restrictions.disjunction();
		matchDisjunction.add(Restrictions.like("kewords", keyword,
				MatchMode.ANYWHERE));
		criteria.add(matchDisjunction);
		List<Article> articleList = criteria.list();
		tx.commit();
		session.close();
		return articleList;
	}

	@SuppressWarnings("unchecked")
	@Override
	public List<Article> getEntityListByDateRange(Date fromDate, Date toDate)
			throws Exception {
		session = sessionFactory.openSession();
		tx = session.beginTransaction();
		Criteria criteria = session.createCriteria(Article.class);
		criteria.add(Restrictions.ge("publishDate", fromDate));
		criteria.add(Restrictions.lt("publishDate", toDate));
		List<Article> articleList = criteria.list();
		tx.commit();
		session.close();
		return articleList;
	}
	
}

/Comment/
