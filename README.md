# myproject
This repo contains the files for the Getting and Cleaning Data Course Project

#Step 1
#Set the appropiate working directory which must contain the project data and assign them to dataframes X_test and X_train.

setwd("C:/Users/lbrag/Desktop/R_Tutotials/data/dataset/train")
X_train<-read.table("X_train.txt",sep="")
subject_train<-read.table("subject_train.txt",sep="")
X_train<-cbind(X_train,subject_train)

setwd("C:/Users/lbrag/Desktop/R_Tutotials/data/dataset/test")
X_test<-read.table("X_test.txt",sep="")
subject_test<-read.table("subject_test.txt",sep="")
X_test<-cbind(X_test,subject_test)

setwd("C:/Users/lbrag/Desktop/R_Tutotials/data/dataset")
features<-read.table("features.txt")

#Merges both dataframes.

data<-rbind(X_test,X_train)
remove(list=c("X_test","X_train","subject_test","subject_train"))

#Step 2: Selects columns that estimates means and standard deviations.
select_cols<-grep("mean|std",features[,2])
select_cols<-c(select_cols,562)
data<-data[,select_cols]

#Step 3 and 4: Gives appropriate labels to variables
library(data.table)

name_new<-as.character(paste(features[,2]))
name_new<-name_new[select_cols]
name_new0<-gsub("\\()","",name_new,ignore.case=TRUE)
name_new<-gsub("\\-","",name_new0)
name_new<-tolower(name_new)
name_new<-sub("bodybody","body",name_new)
name_old<-names(data)
setnames(data, old = name_old, new = name_new)
nm0<-names(data)
nm1<-c(nm0[1:79],"id")
colnames(data)<-nm1

remove("features")

#Step 5 Tidy data

tidy<- data %>% group_by(id) %>% summarize_all(mean)

write.csv(tidy,"C:/Users/lbrag/Desktop/R_Tutotials/data/tidy.csv", row.names = TRUE)
